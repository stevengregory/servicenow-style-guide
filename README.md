# ServiceNow: Style Guide

A modern and opinionated style guide designed for ServiceNow development teams. This guide focuses on promoting clean, efficient, and maintainable code that adheres to best practices.

## Table of Contents

### General Principles

- [Single Responsibility](#single-responsibility)
- [Don't Repeat Yourself (DRY)](#dont-repeat-yourself-dry)

### Design Patterns & Anti-Patterns

- [Revealing Module Pattern](#revealing-module-pattern)
- [God Object (Anti-Pattern)](#god-object-anti-pattern)
- [Dead Code (Anti-Pattern)](#dead-code-anti-pattern)

### Scripting

- [Pure Functions](#pure-functions)
- [Small Functions](#small-functions)
- [Function Declarations](#function-declarations)
- [GlideQuery](#glidequery)

### Service Portal

- [$onInit](#oninit)
- [controllerAs Syntax](#controlleras-syntax)
- [Bindable Members at Top](#bindable-members-at-top)
- [One-time Binding](#one-time-binding)
- [Modules](#modules)
- [AngularJS Components](#angularjs-components)

### UI Framework (Seismic)

- [Component File Structure](#component-file-structure)

### Testing & Quality

- [Unit Testing](#unit-testing)
- [Linting](#linting)
- [Code Format](#code-format)

## Single Responsibility

The [Single Responsibility Principle (SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle) states:

Every class, module, or function in a program should be focused and have one responsibility.

> Each software module should have one and only one reason to change. - Robert C. Martin

In gist, any given unit of code should have only one job and do it well. If the responsibility is scattered across different areas, it creates more touchpoints and increases the probability of creating bugs with future changes.

Apply the single responsibility principle (SRP) to all functions, scripts, components, and classes. Why use SRP?

- Creates more readable code
- Improves maintainability
- Creates more modular code
- Reduces coupling
- More testable

**[Back to top](#table-of-contents)**

## Don't Repeat Yourself (DRY)

The [Don't Repeat Yourself (DRY)](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) principle aims to reduce repetition of code which is likely to change, replacing it with abstractions that are less likely to change, or using data normalization which avoids redundancy in the first place.

> Every piece of knowledge must have a single, unambiguous, authoritative representation within a system. - The Pragmatic Programmer

In summary, creating duplicate code is generally considered an anti-pattern and should be avoided.

Use the DRY principle broadly when developing on the platform. Why follow it?

- Easy to read and understand
- Easy to maintain
- More reusable
- More testable
- Less error-prone

**[Back to top](#table-of-contents)**

## Revealing Module Pattern

The Revealing Module Pattern in JavaScript is a design pattern that encapsulates private variables and methods within a module, exposing only desired methods and properties. With this pattern, only a public API is returned, keeping everything else within the closure private. Use this pattern in your scripts. Why?

- Creates encapsulation
- Improves code clarity & readability
- Enhances code organization
- Improves maintainability

```javascript
var AppCenterUtils = (function () {
  function addUser() {
    /* */
  }

  function doTransform() {
    /* */
  }

  function getApps() {
    /* */
  }

  return {
    addUser: addUser,
    getApps: getApps
  };
})();
```

In this script include, the `addUser` & `getApps` methods are exposed and made public, providing a clear and controlled API. The `doTransform` method, however, is kept private within the module, demonstrating the encapsulation this pattern provides.

**[Back to top](#table-of-contents)**

## God Object (Anti-Pattern)

The [God Object](https://en.wikipedia.org/wiki/God_object) is an anti-pattern in object-oriented programming. It refers to an object that knows too much or does too much. The God Object is usually a class or module that has grown to such proportions that it becomes unmanageable and difficult to maintain.

> A God object is an object that controls too many other objects in the system and has grown beyond all logic to become The Class That Does Everything. - Robert C. Martin

In essence, a God Object is a violation of the [Single Responsibility Principle (SRP)](#single-responsibility). It's an object that has taken on responsibilities that should be delegated to other, more specialized objects.

Avoid creating God Objects in your code. Why and what is the cost?

- Tight coupling
- Difficult to maintain
- Decreased code readability
- Harder to test

When you find a God Object in your code, consider refactoring it into smaller, more focused units of abstraction. This process is often referred to as "breaking down a God Object".

**[Back to top](#table-of-contents)**

## Dead Code (Anti-Pattern)

[Dead code](https://en.wikipedia.org/wiki/Dead_code) is code that is no longer used by your application. It can come in different forms, but most apparent in commented-out code. Leaving dead code in your codebase can have a number of negative consequences, including:

- Decreased code readability
- Hurts maintainability
- Makes it harder to find & fix bugs
- Negatively impacting performance

First & foremost, all of your code should be stored in version control. So instead of commenting out code for potential future use, simply remove it. Look to the version history for reference.

To help eliminate unused variables, functions, and function parameters, add the [no-unused-vars](https://eslint.org/docs/latest/rules/no-unused-vars) rule to your ESLint configuration file.

```yml
rules:
  no-unused-vars: error
```

**[Back to top](#table-of-contents)**

## Pure Functions

Write [pure functions](https://en.wikipedia.org/wiki/Pure_function) when possible. These functions, when given the same input, will always return the same output. They produce no side effects. Pure functions are completely independent of outside state. Why use these?

- Easy to understand
- Easy to maintain
- Reduces bugs
- More testable

```javascript
const double = (x) => x * 2;
```

**[Back to top](#table-of-contents)**

## Small Functions

Write small functions. Do refactor large functions to smaller ones. Why?

- Creates more readable code
- Easy to maintain
- Promotes code reuse
- Easy to test

**[Back to top](#table-of-contents)**

## Function Declarations

As a general rule, use function declarations over function expressions. Why?

- Creates more readable code
- Keeps bindable members at the top
- Hides implementation details
- No concerns using a function before it is defined

```javascript
api.controller = function (coreService) {
  var c = this;
  c.formatDate = formatDate;

  function formatDate(date) {
    return coreService.formatDate(date);
  }
};
```

Avoid using function expressions with `$scope` in your client controller.

```javascript
api.controller = function ($scope, coreService) {
  var c = this;

  $scope.formatDate = function (date) {
    return coreService.formatDate(date);
  };
};
```

**[Back to top](#table-of-contents)**

## GlideQuery

Use [GlideQuery](https://docs.servicenow.com/bundle/tokyo-application-development/page/app-store/dev_portal/API_reference/GlideQuery/concept/GlideQueryGlobalAPI.html) as an interface to the Now platform. `GlideQuery` offers a modern, functional, and safer alternative to `GlideRecord`. Why?

- Creates readable & expressive code
- Simplifies queries
- Easy to catch bugs

```javascript
function userExists(userID) {
  return new GlideQuery('sys_user')
    .where('user_name', userID)
    .selectOne('user_name')
    .isPresent();
}
```

When used within a scoped app, it must be prefixed with the global scope.

```javascript
new global.GlideQuery('sys_user');
```

**[Back to top](#table-of-contents)**

## $onInit

Use the `$onInit` method to organize initialization code for your client controller.

From the AngularJS documentation:

> Called on each controller after all the controllers on an element have been constructed and had their bindings initialized (and before the pre & post linking functions for the directives on this element).

```javascript
api.controller = function () {
  var c = this;

  c.$onInit = function () {
    getTopics();
    getWidgets();
  };
};
```

**[Back to top](#table-of-contents)**

## controllerAs Syntax

Use `controllerAs` syntax instead of `$scope` inside your client controller. Why?

- It is a common best practice
- Removes scope inheritance issues
- Eliminates having to inject `$scope` as a dependency

```javascript
api.controller = function () {
  var c = this;
  c.isVisible = true;
};
```

Avoid using `$scope` in a client controller unless needed.

```javascript
api.controller = function ($scope) {
  $scope.isVisible = true;
};
```

Using `$scope` is fine when publishing and subscribing to events such as: `$emit`, `$broadcast`, or `$on`.

**[Back to top](#table-of-contents)**

## Bindable Members at Top

Place the bindable members at the top of the client controller in alphabetical order. Why?

- Easy to read
- Quickly identify bindings on the HTML template

```javascript
api.controller = function () {
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

The syntax can easily be applied inside an `ng-repeat` as well:

```html
<ul>
  <li ng-repeat="user in ::c.users"></li>
</ul>
```

Note: be careful using one-time bindings in areas where the data could fluctuate in the future.

**[Back to top](#table-of-contents)**

## Modules

Set a module only once in its own file. Why?

- Separates configuration from module definition
- Provides an identifiable place to set module configuration

Do this to define a module in a UI script.

```javascript
angular.module('employee-portal', []);
```

Get instances of the module this way.

```javascript
angular.module('employee-portal');
```

**[Back to top](#table-of-contents)**

## AngularJS Components

In Service Portal, use components to construct independent and reusable bits of code. Components can be registered with the `.component()` helper method. Why use components?

- Simple configuration
- Promotes modular code
- Optimized for component-based architecture

```javascript
(() => {
  const hamburgerMenu = {
    template: [
      '<button type="button" data-target="#nav-bar">',
      '<span class="sr-only">${Toggle nav}</span>',
      '<span class="icon-bar"></span>',
      '<span class="icon-bar"></span>',
      '<span class="icon-bar"></span>',
      '</button>'
    ].join('')
  };

  angular
    .module('employee-portal')
    .component('hamburgerMenu', hamburgerMenu);
})();
```

This code was crafted using a UI script.

**[Back to top](#table-of-contents)**

## Component File Structure

In the [UI Framework](https://developer.servicenow.com/dev.do#!/reference/next-experience/vancouver/ui-framework/getting-started) (Seismic), as the complexity of a component increases, it's a good practice to start abstraction. A good start is moving your view and component logic to stand-alone files. Why?

- Creates a separation of concerns
- Improves maintainability
- Increases code readability

Here is an example component directory structure.

```text
src/
├── example-component/
│   ├── __tests__/
│   ├── component.js
│   ├── index.js
│   ├── styles.scss
│   └── view.js
└── index.js
```

**[Back to top](#table-of-contents)**

## Unit Testing

Unit testing is essential for early issue detection, cost-saving, and efficient development. ServiceNow utilizes [Jasmine](https://jasmine.github.io/), a behavior-driven framework for JavaScript testing. Why write unit tests?

- Ensures code reliability
- Facilitates safe refactoring
- Simplifies debugging
- Accelerates development

A simple function to test:

```javascript
const add = (a, b) => a + b;
```

The corresponding Jasmine unit test:

```javascript
describe('addition', function () {
  it('should add two numbers correctly', function () {
    const result = add(5, 5);
    expect(result).toBe(10);
  });
});
```

**[Back to top](#table-of-contents)**

## Linting

[ESLint](https://eslint.org/) helps you find and fix problems with your JavaScript code. Use it to lint your code. Why?

- Increases code quality
- Makes code more consistent
- Enforces best practices & patterns of development
- Helps catch bugs

The [curly](https://eslint.org/docs/latest/rules/curly) rule is one worth having. As a best practice: avoid omitting curly braces around blocks, it reduces code clarity and can lead to bugs.

```javascript
if (isDuplicate) nowGR.deleteRecord();
```

This statement can be rewritten as such:

```javascript
if (isDuplicate) {
  nowGR.deleteRecord();
}
```

**[Back to top](#table-of-contents)**

## Code Format

[Prettier](https://prettier.io/) is an opinionated code formatter. Use it to automate code formatting. Why?

- Increases code readability
- Makes code more consistent
- Saves you time and energy
- Removes style discussion in code reviews

Here is a sample configuration for your codebase.

```yml
singleQuote: true
semi: true
tabWidth: 2
trailingComma: 'es5'
```

**[Back to top](#table-of-contents)**
