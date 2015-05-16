## How to use Underscore (Lodash) in Angular View

[Underscore](http://underscorejs.org/) (or its drop-in replacement [Lodash](http://lodash.com/)) is a fantastic utility library that provides functional programming constructs such as `map`, `reduce`, `filter`, `any`, `every`, etc.

I use it in pretty much all of my projects and they are super useful especially for data manipulation. Angular project is not an exception.

Since Lodash is attached to window object (i.e. `window._`), which is global, I can use it in my Angular controller without a problem. However, inside Angular view, you can't just access it like this:

```html
<div ng-repeat="item in _.range(1, 11)">
    {{ item }}
</div>
```

Why is it so? Because whatever you're using in Angular view should be available in the corresponding `$scope` (or its ancestor scope).

So you can have an access to `_` in the following way.

```javascript
app.controller('SomeController', function($scope) {
    $scope._ = _;
});
```

Better yet, since `_` is a utility library that's useful globally, you can just attach it to `$rootScope` in Angular's run block and use it in any view.

```javascript
app.run(function ($rootScope) {
    $rootScope._ = window._;
});
```

Using `_` in Angular view can make your code concise and beautiful. Hope this tip helps.
