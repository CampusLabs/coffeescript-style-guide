## Table of Contents

* [Source Code Layout](#source-code-layout)

## Source Code Layout

* Use `UTF-8` as the source file encoding.
* Use two **spaces** per indentation level.

    ```Coffeescript
    # good
    some_method: ->
      do_something

    # bad - four spaces
    some_method: ->
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
    some_method: ->
      data = initialize(options)

      data.manipulate

      data.result
    end

    some_other_method: ->
      result
    end
    ```

* Keep lines fewer than 80 characters.
* Avoid trailing whitespace.
