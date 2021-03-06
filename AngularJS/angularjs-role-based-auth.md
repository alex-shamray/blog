### AngularJS -- Quick Role-Based Authentication

We've recently started on a new product at [work](http://www.learndot.com) using the [AngularJS framework](http://www.angularjs.org). One of the most useful resources when starting out was the *#angularjs* room on [freenode](http://freenode.net/). There seems to be a really large, vibrant community around AngularJS and that's always helpful when starting from scratch on a new framework. One of the best resources in the room was [David Mosher](https://twitter.com/dmosher), who helped us get set up with [Lineman](http://linemanjs.com) -- an awesome build framework based on [Grunt](http://gruntjs.com/) -- and pointed us to some of his [video tutorials](http://www.youtube.com/user/vidjadavemo). Honestly, these were a great resource for getting started and I highly recommend watching them. The following tutorial is heavily inspired by the examples David provided.

#### Basic Routing

Before starting with authentication you'll need to have some routes. Let's start with a logged out state, and a logged in state:

```javascript
angular.module("yourApp").config(function ($routeProvider) {

    $routeProvider.when('/login', {
        templateUrl: 'login.html',
        controller: 'LoginController'
        resolve: {
            //...
        }
    });

    $routeProvider.when('/app', {
        templateUrl: 'app.html',
        controller: 'AppController'
        resolve: {
            //...
        }
    });

});
```

In this simple example we have a state for logging in and another state for seeing the app once logged in. In order to limit access to authenticated users we are going to have to check their status before allowing them to view the *app* state.

#### Adding Authentication

This is the real meat of the solution. They key is adding this new function that watches the `$routeChangeStart` event and reacts on it. Once this event fires, we check if the route is clean (using some fancy [underscore.js](http://underscorejs.org/)). It's easier to enumerate all the routes that don't require rather than all the ones that do. Presumably, there are significantly more that do.

```javascript
angular.module('yourApp').run(function ($rootScope, $location, AuthenticationService) {

    // enumerate routes that don't need authentication
    var routesThatDontRequireAuth = ['/login'];

    // check if current location matches route
    var routeClean = function (route) {
        return _.find(routesThatDontRequireAuth,
            function (noAuthRoute) {
                return _.str.startsWith(route, noAuthRoute);
            });
    };

    $rootScope.$on('$routeChangeStart', function (event, next, current) {
        // if route requires auth and user is not logged in
        if (!routeClean($location.url()) && !AuthenticationService.isLoggedIn()) {
            // redirect back to login
            $location.path('/login');
        }
    });
});
```

As you can see it's fairly intuitive. If the route needs to be authenticated and the user is not logged in, then redirect them back to the login page. This doesn't necessarily have to be a redirect, this can be an API request to see if their session cookie is still active. You can build on this and make it as sophisticated as you need -- but the skeleton will probably remain the same.

#### Handling Failed Authentication from the API

There is another possibility, of course, in which the user is not authenticated and initiates an action that results in API request. The result will be a *401* response and possibly some sort of authentication exception. Handling this *401* should be done as follows (all credit goes to [David Mosher](https://twitter.com/dmosher) on this one):

```javascript
angular.module('yourApp').config(function ($httpProvider) {

    var logsOutUserOn401 = ['$q', '$location', function ($q, $location) {
        var success = function (response) {
            return response;
        };

        var error = function (response) {
            if (response.status === 401) {
                //redirect them back to login page
                $location.path('/login');

                return $q.reject(response);
            }
            else {
                return $q.reject(response);
            }
        };

        return function (promise) {
            return promise.then(success, error);
        };
    }];

    $httpProvider.responseInterceptors.push(logsOutUserOn401);
});
```

As you can see, we add an interceptor to capture the promises from the `$httpProvider` and validate them to make sure they are not *401* errors. If they happen to be *401* then we send the user back to the login page.

#### Mixing in UI-Router

One of the more useful plugins for angular is [ui-router](https://github.com/angular-ui/ui-router). This plugin allows you to create more powerful constructs with your states by using inheritance and substate transitions. Using *ui-router*, our route definition would be as follows:

```javascript
angular.module('yourApp').config(function ($stateProvider, $urlRouterProvider) {

    $urlRouterProvider.otherwise('/');

    $stateProvider
        .state('login', {
            url: '/login',
            templateUrl: 'login.html',
            controller: 'LoginController'
            resolve: {
                //...
            }
        })

        .state('app', {
            url: '/app',
            templateUrl: 'app.html',
            controller: 'AppController'
            resolve: {
                //...
            }
        });
});
```

Similarly, we'll need to make some minor changes to the authentication watch function -- from watching route change, to state change.

```javascript
angular.module('yourApp').run(function ($rootScope, $location, AuthenticationService) {

    // enumerate routes that don't need authentication
    var routesThatDontRequireAuth = ['/login'];

    // check if current location matches route
    var routeClean = function (route) {
        return _.find(routesThatDontRequireAuth,
            function (noAuthRoute) {
                return _.str.startsWith(route, noAuthRoute);
            });
    };

    $rootScope.$on('$stateChangeStart', function (ev, to, toParams, from, fromParams) {
        // if route requires auth and user is not logged in
        if (!routeClean($location.url()) && !AuthenticationService.isLoggedIn()) {
            // redirect back to login
            $location.path('/login');
        }
    });
});
```

Fortunately, we don't have to change the 401 intercepter and everything works just as before.

#### Expanding into Role-Based Authentication

The last step that you may want to take is role-based authentication. This is starting to add a bit more complexity to your app, and may not be necessary on the JS side. However, if you do need it, it's a critical component.

The way to approach this would be have some sort of user object with roles, ideally specified in an injectable service:

```javascript
angular.module('yourApp').factory('UserService', function () {

    var currentUser = null;

    var adminRoles = ['admin', 'editor'];
    var otherRoles = ['user'];

    return {
        // some code that gets and sets the user to the singleton variable...

        validateRoleAdmin: function () {
            return _.contains(adminRoles, currentUser.role);
        },

        validateRoleOther: function () {
            return _.contains(otherRoles, currentUser.role);
        }
    };
});
```

Then inject this `UserService` into the authentication watcher for further authorization, after the authentication step. Or, probably more ideally, you would want to create an `AuthorizationService` and separate it from the `UserService` (i.e. separation of concerns). It would look something like this:

```javascript
angular.module('yourApp').run(function ($rootScope, $location, AuthenticationService, UserService) {

    // enumerate routes that don't need authentication
    var routesThatDontRequireAuth = ['/login'];
    var routesThatForAdmins = ['/admin'];

    // check if route does not require authentication
    var routeClean = function(route) { //... }
    // check if route requires admin priviledge
    var routeAdmin = function(route) { //... }

    $rootScope.$on('$stateChangeStart', function (ev, to, toParams, from, fromParams) {
        if (!routeClean($location.url()) && !AuthenticationService.isLoggedIn()) {
            // redirect back to login
            $location.path('/login');
        }
        else if (routeAdmin($location.url() && !UserService.validateRoleAdmin()) {
            // redirect to error page
            $location.path('/error');
        }
    });
});
```

#### Update

I have had quite a few requests for a working code sample for this post. I spent some time putting one together and you can find it [here](https://github.com/artgon/angularjs-role-based-auth). Note that the working code is slightly different than the code above, but the main idea is the same.

#### Source

"AngularJS -- Quick Role-Based Authentication", Arthur Gonigberg (29.06.2013) [http://arthur.gonigberg.com/2013/06/29/angularjs-role-based-auth/](http://arthur.gonigberg.com/2013/06/29/angularjs-role-based-auth/)
