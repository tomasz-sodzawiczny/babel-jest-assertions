<div align="center">
  <h1>babel-jest-assertions</h1>

  🃏⁉️

  Adds expect.assertions(n) and expect.hasAssertions to all tests automatically
</div>

<hr />

[![Build Status](https://img.shields.io/travis/mattphillips/babel-jest-assertions.svg?style=flat-square)](https://travis-ci.org/mattphillips/babel-jest-assertions)
[![Code Coverage](https://img.shields.io/codecov/c/github/mattphillips/babel-jest-assertions.svg?style=flat-square)](https://codecov.io/github/mattphillips/babel-jest-assertions)
[![version](https://img.shields.io/npm/v/babel-jest-assertions.svg?style=flat-square)](https://www.npmjs.com/package/babel-jest-assertions)
[![downloads](https://img.shields.io/npm/dm/babel-jest-assertions.svg?style=flat-square)](http://npm-stat.com/charts.html?package=babel-jest-assertions&from=2017-09-14)
[![MIT License](https://img.shields.io/npm/l/babel-jest-assertions.svg?style=flat-square)](https://github.com/mattphillips/babel-jest-assertions/blob/master/LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

## Problem

Ever wondered if you test is actually running the assertions, especially in asynchronous tests? Jest has two features
built in to help with this: [`expect.assertions(number)`](https://facebook.github.io/jest/docs/en/expect.html#expectassertionsnumber)
and [`expect.hasAssertions()`](https://facebook.github.io/jest/docs/en/expect.html#expecthasassertions). These can be
useful when doing something like:

```js
it('resolves to one', () => {
  Promise.reject(1).then(value => expect(value).toBe(1));
});
```

The issue here is the `catch` case is not dealt with in this test, _which is fine_, but this test will currently pass
even though the `Promise` rejects and the assertion is never ran.

## Solution

One solution is to manually adjust the above test to include `expect.assertions(number)` and `expect.hasAssertions()`.
However this is quite verbose and prone to human error.

An alternative is a babel plugin to automate adding these additional properties, and this is such plugin 😉.

## Installation

With npm:
```sh
npm install --save-dev babel-jest-assertions
```

With yarn:
```sh
yarn add -D babel-jest-assertions
```

## Setup

### .babelrc

```json
{
  "plugins": ["babel-jest-assertions"]
}
```

### CLI

```sh
babel --plugins babel-jest-assertions script.js
```

### Node

```javascript
require('babel-core').transform('code', {
  plugins: ['babel-jest-assertions'],
})
```

## Usage

Simply write your tests as you would normally and this plugin will add the verification of assertions in the background.

```js
it('resolves to one', () => {
  Promise.reject(1).then(value => expect(value).toBe(1));
});
```

`↓ ↓ ↓ ↓ ↓ ↓`

```js
it('resolves to one', () => {
  expect.hasAssertions();
  expect.assertions(1);
  Promise.reject(1).then(value => expect(value).toBe(1));
});
```

### Override

If you add either `expect.assertions(number)` or `expect.hasAssertions()` then your defaults will be favoured and the
plugin will skip the test.

## Contributors

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
| [<img src="https://avatars0.githubusercontent.com/u/5610087?v=4" width="100px;"/><br /><sub>Matt Phillips</sub>](http://mattphillips.io)<br />[💻](https://github.com/mattphillips/babel-jest-assertions/commits?author=mattphillips "Code") [📖](https://github.com/mattphillips/babel-jest-assertions/commits?author=mattphillips "Documentation") [🚇](#infra-mattphillips "Infrastructure (Hosting, Build-Tools, etc)") [⚠️](https://github.com/mattphillips/babel-jest-assertions/commits?author=mattphillips "Tests") |
| :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

## LICENSE

MIT
