# Angular JS Testing with Jasmine

## Jasmine useful functions

```
describe
beforeEach
afterEach
it
expect
```

## Jasmine Matchers
Name|Description
-----|----------
expect(x).toEqual(val) | Asserts that x has the same value as val
expect(x).toBe(obj) | Asserts that x and obj are the same object
expect(x).toMatch(regexp) | Asserts that x matches the specified regular expression
expect(x).toBeDefined() | Asserts that x has been defined
expect(x).toBeUndefined() | Asserts that x has not been defined
expect(x).toBeNull() | Asserts that x is null
expect(x).toBeTruthy() | Asserts that x is true or evaluates to true
expect(x).toBeFalsy() | Asserts that x is false or evaluates to false
expect(x).toContain(y) | Asserts that x is a string that contains y
expect(x).toBeGreaterThan(y) | Asserts that x is greater than y

> For negation use something like this expect(x).not.toEqual(val)

## Setup Karma

```
npm install karma -g
karma init karma.config.js
karma start karma.config.js ( any change will run test)
or
karma run karma.config.js ( run once, no watch )
```


