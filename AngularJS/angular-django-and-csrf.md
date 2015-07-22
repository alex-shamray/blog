# angular, django and csrf

If you use AngularJS 1.1.3 or newer you can use `xsrfHeaderName` and `xsrfCookieName`:

```javascript
var myapp = angular.module('myapp', ['ngCookies', 'ui.bootstrap']).
    config(['$routeProvider', function ($routeProvider, $httpProvider, $cookies) {
        $httpProvider.defaults.xsrfHeaderName = 'X-CSRFToken';
        $httpProvider.defaults.xsrfCookieName = 'csrftoken';
        ...
```

See [$httpProvider](https://code.angularjs.org/1.4.3/docs/api/ng/provider/$httpProvider) in 1.4.3 the documentation.
