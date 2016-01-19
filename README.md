# Using controllerAs

## Overview

We've learned already what `$scope` is and the magic it provides with our view. However, before we start going `$scope` mad, it's important we learn about the best practices when it comes to controllers and our views.

## Objectives

- Controller as Classes
- Use the "this" keyword instead of $scope
- Instantiate the Controller against an instance variable

## What is controllerAs?

As you've learned, controllers revolve around the mysterious `$scope` object. However, there is a much cleaner approach to writing our controllers.

Instead of injecting `$scope` into every controller, we can simply just use the `this` keyword and assign items to that instead.
 
There's a slight change in our `ng-controller` syntax too:

```html
<div ng-controller="MainController as main">
</div>
```

Here we tell Angular we'd like to use our controller *as* a variable named `main`. This then creates a new instance of our controller (with all of our values bound to the controller's instance, rather than `$scope`) and assigns the instance to a variable named `main`. This then allows us to access our properties with ``{{ main.propertyName }}`.

We'd take this:

```js
function MainController($scope) {
  $scope.name = 'Bill Gates';
}
```

And convert it into:

```js
function MainController() {
  this.name = 'Bill Gates';
}
```

Done! That's all it takes.

## Oh no, nested scopes

This helps us when we have nested scopes. If we had a controller inside a controller, and both added the variable `name` to `$scope` - what one would we see?

```html
<div ng-controller="MainController">
  {{ name }} <!-- This is from MainController -->
  <div ng-controller="AnotherController">
    {{ name }} <!-- Where's this from? -->
  </div>
</div>
```

We wouldn't be sure where `name` was coming from in this example. What if we wanted to use `name` from `MainController` instead of `AnotherController` both times? We wouldn't be able to!

That's where controllerAs saves the day - we can assign each controller to different variables and we'd know where `name` is from.
 
```html
<div ng-controller="MainController as main">
  {{ main.name }}
  <div ng-controller="AnotherController as another">
    {{ another.name }} and {{ main.name }}
  </div>
</div>
```

## What about the rest of $scope

So does this mean we can use `this.$watch` instead of `$scope.$watch`? Unfortunately not - the API for watching value changes still sits with `$scope`, so we still have to use that.

Think of it this way - when we use controllerAs, we're allowing our controllers to assign anything they want our view to access to the `this` keyword. Everything else is as it was before!