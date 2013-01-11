## Table of Contents

* [Source Code Layout](#source-code-layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Comments](#comments)
* [Annotations](#annotations)
* [Classes](#classes)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Misc](#misc)
* [Referencing DOM](#referencing-dom)

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
  should start with `is` or `has`.
  (i.e. `isEmpty`, `hasDefaultValue`).

## Comments

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell

* Remember that If the file has an initial comment block it will not be removed
  when the file is compressed. No sensitive information should be put in the
  initial block.
* Any publically released code should contain a license block at the start
  of the file.
* Gem spec style information should be included in a block at the start of the
  file (after the license if needed). Version and homepage information are
  important for publically released code. Internally, dependencies can be helpful
  in determining weather a JavaScript/CoffeeScript library is being used elsewhere.

    ```Coffeescript
    # version: 1.1.0
    # homepage: https://github.com/orgsync/some_lib
    # dependency: jQuery, ~> 1.7.1
    ```
    
* Write self-documenting code and ignore the rest of this section. Seriously!
* Comments longer than a word are capitalized and use punctuation. Use [one
  space](http://en.wikipedia.org/wiki/Sentence_spacing) after periods.
* Avoid superfluous comments.

    ```Coffeescript
    # bad
    counter += 1 # increments counter by one
    ```

* Keep existing comments up-to-date. An outdated comment is worse than no
  comment at all.

> Good code is like a good joke - it needs no explanation. <br/>
> -- Russ Olsen

* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Do or do not - there is no try. --Yoda)

## Annotations

* Annotations should usually be written on the line immediately above
  the relevant code.
* The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
* If multiple lines are required to describe the problem, subsequent
  lines should be indented two spaces after the `#`.

    ```Coffeescript
    bar = ->
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz('quux')
    ```

* In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```Coffeescript
    bar = ->
      @sleep(100) # OPTIMIZE
    ```

* Use `TODO` to note missing features or functionality that should be
  added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
* Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`
* Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.

## Classes

* When designing class hierarchies make sure that they conform to the
  [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle).
* Try to make your classes as
  [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))
  as possible.
* Prefer [duck-typing](http://en.wikipedia.org/wiki/Duck_typing) over inheritance.

    ```Coffeescript
    # bad
    class Animal
      # abstract method
      speak: ->

    # extend superclass
    class Duck extends Animal
      speak: ->
        alert 'Quack! Quack'

    # extend superclass
    class Dog extends Animal
      speak: ->
        alert 'Bau! Bau!'

    # good
    class Duck
      speak: ->
        alert 'Quack! Quack'

    class Dog
      speak: ->
        alert 'Bau! Bau!'
    ```

* Denote private methods and attributes with `_`.

## Strings

* Prefer string interpolation instead of string concatenation:

    ```Coffeescript
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.

    ```Coffeescript
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Prefer single-quoted strings when you don't need string interpolation or
  special symbols such as `\t`, `\n`, `'`, etc.

    ```Coffeescript
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```
    
## Regular Expressions

* Don't use regular expressions if you just need plain text search in string:
  `'text'.match('ex')`

* Use non capturing groups when you don't use captured result of parenthesis.

    ```Coffeescript
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* Use `///` modifier for complex regexps. This makes them more readable and you
  can add some useful comments. Just be careful as spaces are ignored.

    ```Coffeescript
    regexp = ///
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    ///
    ```
    
## Misc

* Avoid hashes as optional parameters. Does the method do too much?
* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.
* Code in a functional way, avoiding mutation when that makes sense.
* Avoid needless metaprogramming.
* Do not mutate arguments unless that is the purpose of the method.
* Avoid more than three levels of block nesting.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.

## Referencing DOM

* A class should never be used for both scripting and CSS. Classes used in scripting to find DOM
  elements must use the prefix `js-`.
