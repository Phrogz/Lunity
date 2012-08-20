## Lunity v0.9.1 

A simple single-file unit test system for Lua with a somewhat rich set of assertions and custom error messages. Features:

* Lua 5.1 module
* Framework is a single file.
* Define utility functions separate from and available to test functions.
* Control the order that tests are run (by renaming functions).
* Can output ANSI (for conforming terminals) or HTML (for TextMate or e).
* Sensible (to me) injection of functions while preserving global scope.
* Rich set of assertions and custom error messages.  
  (Including assertion for comparing tables of values.)


## Simplest Usage

<!-- language: lang-lua -->

     require 'test/lunity'
     module( 'TEST_THE_WORLD', lunity )

     function test1_hello_world
       assert( true, "If true isn't true, we're in big trouble." )
     end


## Robust Usage

<!-- language: lang-lua -->

     require 'test/lunity'
     module( 'TEST_RUNTIME', lunity )
     
     function setup()
       -- code here will be run before each test
     end
     
     function teardown()
       -- code here will be run after each test
     end
     
     -- Tests to run must either start with or end with 'test'
     function test1_foo()
       assertTrue( 42 == 40 + 2 )
       assertFalse( 42 == 40 )
       assertEqual( 42, 40 + 2 )
       assertNotEqual( 42, 40, "These better not be the same!" )
       assertTableEquals( { a=42 }, { ["a"]=6*7 } )
       -- See below for more assertions available
     end
     
     function test2_bar()
       -- Tests will be run in alphabetical order of the entire function name
     end
     
     function some_utility()
       -- You can define helper functions for your tests to call with impunity
     end
     
     runTests(  )
     -- or runTests{ useANSI = false }
     -- or runTests{ useHTML = true  }

## Assertions Available

_The `msg` parameter is always optional; some effort has been made to provide a somewhat helpful message if no message is provided._

* **`fail( msg )`** - instantly fail the test
* **`assert( testCondition, msg )`** - fail if `testCondition` is either `nil` or `false`
* **`assertEqual( actual, expected, msg )`** - fail if `actual ~= expected`
* **`assertNotEqual( actual, expected, msg )`** - fail if `actual == expected`
* **`assertTableEquals( actual, expected, msg )`** - recursively compare two tables and fail if they have any different keys or values
* **`assertTrue( actual, msg )`** - fail if `actual ~= true`
* **`assertFalse( actual, msg )`** - fail if `actual ~= false`
* **`assertNil( actual, msg )`** - fail if `actual ~= nil`
* **`assertNotNil( actual, msg )`** - fail if `actual == nil`
* **`assertType( actual, expectedType, msg )`** - fail if `type(actual) ~= expectedType`  
  _For example, `assertType(getResults(),"table","getResults() needs to return a table")`_

* **`assertTableEmpty( actual, msg )`** - fail if `actual` is not a table, or if `actual` is a table with any keys (including a key with the value of `false`)
* **`assertTableNotEmpty( actual, msg )`** - fail if `actual` is not a table, or if `actual` does not have any keys
* **`assertInvokable( value, msg )`** - fail if `actual` is not a function and may not be invoked as one (via `meta.__call`)
* **`assertErrors( invokable, ... )`** - call the function `invokable` passing along extra parameters, and fail if no errors are raised
* **`assertDoesNotError( invokable, ... )`** - call the function `invokable` passing along extra parameters, and fail if any errors are raised

### Helper Functions

_The following functions are made available to your tests, though they do not count as assertions and cannot cause a test failure on their own._

* **`is_nil( value )`** - return true if `value` is `nil`
* **`is_boolean( value )`** - return true if `value` is a boolean
* **`is_number( value )`** - return true if `value` is a number
* **`is_string( value )`** - return true if `value` is a string
* **`is_table( value )`** - return true if `value` is a table
* **`is_function( value )`** - return true if `value` is a function
* **`is_thread( value )`** - return true if `value` is a thread
* **`is_userdata( value )`** - return true if `value` is a userdata value

_Note that while writing `assert(is_table(value))` looks nicer in code than `assertType(value,"table")`, the latter provides a better default error message to the user._

## License

This work is licensed under the Creative Commons Attribution 3.0
United States License. To view a copy of this license, visit
http://creativecommons.org/licenses/by/3.0/us/ or send a letter to

    Creative Commons
    171 Second Street, Suite 300
    San Francisco, California, 94105, USA

Modification, reuse and redistribution permitted provided the following
attribution and copyright line is included:

Copyright (c) 2012 by [Gavin Kistner](mailto:!@phrogz.net) (Phrogz)





