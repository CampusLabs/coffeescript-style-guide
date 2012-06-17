## Table of Contents

* [Source Code Layout](#source-code-layout)
* [Syntax](#syntax)
* [Naming](#naming)

## Source Code Layout

* Use `UTF-8` as the source file encoding.
* Use two **spaces** per indentation level.

    ```Coffeescript
    # good
    some_method = ->
      do_something

    # bad - four spaces
    some_method = ->
        do_something
    ```

* Use Unix-style line endings. (*BSD/Solaris/Linux/OSX users are covered by default,
  Windows users have to be extra careful.)
    * If you're using Git you might want to add the following
    configuration setting to protect your project from Windows line
    endings creeping in:

        ```$ git config --global core.autocrlf true```

* Use spaces around operators, after commas and semicolons.

    ```Coffeescript
    sum = 1 + 2
    nums = [1, 2, 3]
    ```

* No spaces after `(`, `[` or before `]`, `)`.

    ```Coffeescript
    some(arg).other
    [1, 2, 3].length
    ```
    
* No spaces after `:` when used in an object.

    ```Coffeescript
    # bad
    song =
      name : 'Misty'
      duration : 192
      year : 1999

    # good
    song =
      name: 'Misty'
      duration: 192
      year: 1999
    ```

* Use empty lines between methods and to break up a method into logical
  paragraphs.

    ```Coffeescript
    some_method = ->
      data = initialize(options)

      data.manipulate

      data.result

    some_other_method = ->
      result
    ```

* Keep lines fewer than 80 characters.
* Avoid trailing whitespace.

## Syntax

* Omit the parentheses on method creation when the method doesn't accept any
  arguments. This also goes for anonymous functions.

     ```Coffeescript
     some_method = ->
       # body omitted

     some_method_with_arguments = (arg1, arg2) ->
       # body omitted
     ```

* Avoid the use of third party (underscore, jQuery) iterators when
  a `for` loop can be used. The iterators add unnecessary overhead.

    ```Coffeescript
    arr = [1, 2, 3]

    # bad
    _.each arr, (elem) ->
      console.log elem

    # good
    console.log elem for elem in arr
    ```

* Never use `then` for multi-line `if/unless`.

    ```Coffeescript
    # bad
    if some_condition then
      # body omitted

    # good
    if some_condition
      # body omitted
    ```

* Favor modifier `if/unless` usage when you have a single-line
  body. Another good alternative is the usage of control flow `and/or`.

    ```Coffeescript
    # bad
    if some_condition
      do_something

    # good
    do_something if some_condition

    # another good option
    some_condition and do_something
    ```

* Favor `unless` over `if` for negative conditions (or control
  flow `or`).

    ```Coffeescript
    # bad
    do_something if !some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition or do_something
    ```

* Never use `unless` with `else`. Rewrite these with the positive case first.

    ```Coffeescript
    # bad
    unless success?
      console.log 'failure'
    else
      console.log 'success'

    # good
    if success?
      console.log 'success'
    else
      console.log 'failure'
    ```

* Don't use parentheses around the condition of an `if/unless/while`,
  unless the condition contains an assignment (see "Using the return
  value of `=`" below).

    ```Coffeescript
    # bad
    if (x > 10)
      # body omitted

    # good
    if x > 10
      # body omitted

    # ok
    if (x = self.next_value)
      # body omitted
    ```

* Favor modifier `for` usage when you have a single-line
  body.

    ```Coffeescript
    # bad
    for elem in elements
      do_something

    # good
    do_something for elem in elements
    ```

* Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Jasmine) and methods that are with  "keyword"
  status in CoffeeScript (e.g. `alert`). Use parentheses around the
  arguments of all other method invocations.

    ```Coffeescript
    class Person
      constructor: (name, age) ->
        @name = name
        @age = age

    temperance = new Person('Temperance', 30)
    temperance.name

    alert temperance.age
    console.log temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```
    
* Omit parenthesis for functions whose final argument is an anonymous function.

    ```Coffeescript
    _.map [1, 2, 3], (num) ->
      num * 3
    ```

* Avoid `return` where not required.

    ```Coffeescript
    # bad
    some_method = (some_arr) ->
      return some_arr.size

    # good
    some_method = (some_arr) ->
      some_arr.size
    ```

* Avoid shadowing methods with local variables unless they are both equivalent

    ```Coffeescript
    class Foo
      constructor: (options) ->
        @options = options

      # bad
      do_something = (options = {}) ->
        unless options['when'] == 'later'
          output(self.options[:message])

      # good
      do_something = (params = {}) ->
        unless params['when'] == 'later'
          output(options[:message])
    ```


* Use spaces around the `=` operator when assigning default values to method parameters:

    ```Coffeescript
    # bad
    some_method = (arg1='default', arg2=null, arg3=[]) ->
      # do something...

    # good
    some_method = (arg1 = :default, arg2 = nil, arg3 = []) ->
      # do something...
    ```

* Using the return value of `=` (an assignment) is ok, but surround the
  assignment with parenthesis.

    ```Coffeescript
    # good - shows intended use of assignment
    if (v = str.indexOf('foo')) ...

    # bad
    if v = str.indexOf('foo') ...

    # also good - shows intended use of assignment and has correct precedence.
    if (v = @next_value) == 'hello' ...
    ```

* Use `or=` freely to initialize variables.

    ```Coffeescript
    # set name to Bozhidar, only if it's null or false
    name or= 'Bozhidar'
    ```

* Don't use `or=` to initialize boolean variables. (Consider what
would happen if the current value happened to be `false`.)

    ```Coffeescript
    # bad - would set enabled to true even if it was false
    enabled or= true

    # good
    enabled = true unless enabled?
    ```
    
## Naming

> The only real difficulties in programming are cache invalidation and
> naming things. <br/>
> -- Phil Karlton

* Use `snake_case` for variables.
* Use `CamelCase` for classes, modules and methods.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)
* Use `SCREAMING_SNAKE_CASE` for other constants.
* The names of predicate methods (methods that return a boolean value)
  should start with `is`.
  (i.e. `isEmpty`).