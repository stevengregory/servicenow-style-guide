# Service Portal: Style Guide

A modern & opinionated style guide for teams using Service Portal.

## Table of Contents

1. [Single Responsibility](#single-responsibility)
1. [Pure Functions](#pure-functions)
1. [controllerAs Syntax](#controlleras-syntax)
1. [$onInit](#oninit)
1. [Bindable Members at Top](#bindable-members-at-top)
1. [One-time Binding](#one-time-binding)

## Single Responsibility

The [Single Responsibility Principle (SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle) states:

Every class, module, or function in a program should be focused and have one responsibility.

> A class should have one and only one reason to change.

In gist, any given unit of code should have only one job and do it well. If the responsibility is scattered across different areas, it creates more touchpoints and increases the probability of creating bugs with future changes.

Apply the single responsibility principle (SRP) to all functions, scripts, components, and classes. Why use SRP?

* Makes the application cleaner
* Creates more modular code
* Easier to read and maintain
* Reduces bug count
* Produces more testable code

**[Back to top](#table-of-contents)**

## Pure Functions

Write [pure functions](https://en.wikipedia.org/wiki/Pure_function) when possible. These functions, when given the same input, will always return the same output. They produce no side effects. Pure functions are completely independent of outside state. Why use these?

* Reduce bugs
* Easy to understand
* Easy to maintain
* More testable

```javascript
const double = x => x * 2;
```

**[Back to top](#table-of-contents)**

## controllerAs Syntax

Use `controllerAs` syntax instead of `$scope` inside your client controller. Why?

* it is a common best practice
* removes scope inheritance issues
* eliminates having to inject `$scope` as a dependency

```javascript
api.controller = function() {
  var c = this;
  c.isVisible = true;
};
```

Avoid using `$scope` in a client controller unless needed.

```javascript
api.controller = function($scope) {
  $scope.isVisible = true;
};
```

Using `$scope` is fine when publishing and subscribing to events such as: `$emit`, `$broadcast`, or `$on`.

**[Back to top](#table-of-contents)**

## $onInit

Use the `$onInit` method to organize initialization code for your client controller.

From the AngularJS documentation:

> Called on each controller after all the controllers on an element have been constructed and had their bindings initialized (and before the pre & post linking functions for the directives on this element).

```javascript
api.controller = function() {
  var c = this;

  c.$onInit = function() {
    getTopics();
    getWidgets();
  };
};
```

**[Back to top](#table-of-contents)**

## Bindable Members at Top

Place the bindable members at the top of the client controller in alphabetical order. Why?

* Easy to read
* Quickly identify bindings on the HTML template

```javascript
api.controller = function() {
  var c = this;
  c.closeSurvey = closeSurvey;
  c.setRating = setRating;

  function closeSurvey() {
    /* */
  }

  function setRating() {
    /* */
  }
};
```

**[Back to top](#table-of-contents)**

## One-time Binding

On the HTML template, use one-time bindings where needed to improve performance.

From the AngularJS documentation:

> One-time expressions will stop recalculating once they are stable, which happens after the first digest if the expression result is a non-undefined value.

```html
<div class="panel-heading">{{::options.title}}</div>
```

The syntax can easily be applied inside an ng-repeat as well:

```html
<ul>
  <li ng-repeat="user in ::c.users"></li>
</ul>
```

Note: Be careful using one-time bindings in areas where the data could fluctuate in the future.

**[Back to top](#table-of-contents)**
