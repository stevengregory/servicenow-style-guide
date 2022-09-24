# Service Portal: Style Guide

A modern style guide for teams using Service Portal.

## Table of Contents

1. [Single Responsibility](#single-responsibility)
1. [Pure Functions](#pure-functions)

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
