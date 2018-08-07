## Lunity v0.11

A simple single-file unit test system for Lua with a somewhat rich set of assertions and custom error messages. Features:

* Lua 5.2,5.3 compatible.
* Framework is a single file.
* Control the order that tests are run (by renaming functions).
* Can output ANSI (for conforming terminals) or HTML (for TextMate or e).
* Sensible (to me) injection of functions while preserving global scope.
* Rich set of assertions and custom error messages.  
  (Including assertion for comparing tables of values.)


## Simplest Usage

``` lua
-- This magic line makes a `test` table available,
-- and makes all the assertions available to your tests
_ENV = require('lunity')()

-- Define tests that you want to run inside the `test` table
function test.simplest_assertion()
  assert(true, "If true isn't true, we are in big trouble.")  
end

-- At the end of your file, call `test()` to run the tests
test()
```

## Robust Usage

``` lua
-- You can optionally give a name to your test suite
_ENV = require('lunity')('check robustness')
 
function test:before()
  -- code here will be run before each test
  -- `self` is available to store information needed for the test
end
     
function test:after()
  -- run after each test, in case teardown is needed
end
     
-- Tests can have any name (other than 'before' or 'after')
-- The `self` table is the same scratchpad available to before()
function test:foo()
  assertTrue(42 == 40 + 2)
  assertFalse(42 == 40)
  assertEqual(42, 40 + 2)
  assertNotEqual(42, 40, "These better not be the same!")
  assertTableEquals({ a=42 }, { ["a"]=6*7 })
  -- See below for more assertions available
end

-- You can define helper functions for your tests to call with impunity
local function some_utility()
  return "excellent"
end

-- Tests will be run in alphabetical order of the entire function name
-- However, relying on the order of tests is an anti-pattern; don't do it
function test:bar()
  assertType(some_utility(), "string")
end

test()
-- or test{ useANSI = false }
-- or test{ useHTML = true  }
```


## Assertions Available

_The `msg` parameter is always optional; effort has been made to provide a helpful message if none is provided._

* <strong>`fail(msg)`</strong> - instantly fail the test
* <strong>`assert(testCondition, msg)`</strong> - fail if `testCondition` is either `nil` or `false`
* <strong>`assertEqual(actual, expected, msg)`</strong> - fail if `actual ~= expected`
* <strong>`assertNotEqual(actual, expected, msg)`</strong> - fail if `actual == expected`
* <strong>`assertTableEquals(actual, expected, msg)`</strong> - recursively compare two tables and fail if they have any different keys or values
* <strong>`assertTrue(actual, msg)`</strong> - fail if `actual ~= true`
* <strong>`assertFalse(actual, msg)`</strong> - fail if `actual ~= false`
* <strong>`assertNil(actual, msg)`</strong> - fail if `actual ~= nil`
* <strong>`assertNotNil(actual, msg)`</strong> - fail if `actual == nil`
* <strong>`assertType(actual, expectedType, msg)`</strong> - fail if `type(actual) ~= expectedType`, e.g. `assertType(getResults(),"table","getResults() needs to return a table")`
* <strong>`assertTableEmpty(actual, msg)`</strong> - fail if `actual` is not a table, or if `actual` is a table with any keys (including a key with the value of `false`)
* <strong>`assertTableNotEmpty(actual, msg)`</strong> - fail if `actual` is not a table, or if `actual` does not have any keys
* <strong>`assertSameKeys(table1, table2, msg)`</strong> - fail if either table has a key not present in the other (good for set comparisons)
* <strong>`assertInvokable(value, msg)`</strong> - fail if `actual` is not a function, or may not be invoked as one (via `meta.__call`)
* <strong>`assertErrors(invokable, ...)`</strong> - call the function `invokable` passing along extra parameters, and fail if no errors are raised
* <strong>`assertDoesNotError(invokable, ...)`</strong> - call the function `invokable` passing along extra parameters, and fail if any errors are raised

### Helper Functions

_The following functions are made available to your tests, though they do not count as assertions and will not cause a test failure on their own._

* <strong>`is_nil(value)`</strong> - return true if `value` is `nil`
* <strong>`is_boolean(value)`</strong> - return true if `value` is a boolean
* <strong>`is_number(value)`</strong> - return true if `value` is a number
* <strong>`is_string(value)`</strong> - return true if `value` is a string
* <strong>`is_table(value)`</strong> - return true if `value` is a table
* <strong>`is_function(value)`</strong> - return true if `value` is a function
* <strong>`is_thread(value)`</strong> - return true if `value` is a thread
* <strong>`is_userdata(value)`</strong> - return true if `value` is a userdata value

*Note that while writing `assert(is_table(value))` looks nicer in code than `assertType(value, 'table')`, the latter provides a better default error message to the user.*


## License

This work is licensed under the Creative Commons Attribution 3.0
United States License. To view a copy of this license, visit
http://creativecommons.org/licenses/by/3.0/us/ or send a letter to

    Creative Commons
    171 Second Street, Suite 300
    San Francisco, California, 94105, USA

Modification, reuse and redistribution permitted provided the following
attribution and copyright line is included:

Copyright (c) 2012-2018 by [Gavin Kistner](mailto:!@phrogz.net) (Phrogz)
