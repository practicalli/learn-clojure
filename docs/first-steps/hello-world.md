# Hello World

Writing code to print out hello world is a common first step with any new language.

Use the `println` function from clojure.core to accomplish this task.

Enter the following code at the REPL prompt:

!!! NOTE ""
    ```clojure
    (println "Hello World")
    ```

Expected result

```clojure
Hello World
nil 
```

`Hello World` is printed, followed by a `nil` value.

!!! HINT "println is a side-effect function"
    `println` is considered a function that creates a side-effect, as it sends information to the standard out process rather than returning a value.

    `nil` is the default return value if an expression does not return a value.


## A Clojure Expression

`()` are used to define a Clojure expression.

`()` means a list of elements, the first element is a call to a function and all other elements are passed as arguments to the function.

??? INFO "Homoiconicity - one represent for code and data"
    Clojure is homoiconic as code and data share the same representation, i.e. use the same iconography.

    A `()` list is a data structure, a collection of data values.

    A `()` list is also used to represent code behaviour (algorithms), e.g calling built-in or custom functions.

    A function call returns a data value (nil is also a value).


## Return a value

Expressions and Function calls always return a value, the `nil` value being returned by default.

An explicit return form is not required, the result of the last expression is returned.

Enter the following code at the REPL prompt:

!!! NOTE ""
    ```clojure
    (str "Hello World")
    ```

Expected result

```clojure
"Hello World"
```

`Hello World` is returned as a data value, instead of the default `nil` return value.

The example is a single expression, so the value created by evaluating the expresion is returned.

`clojure.core/str` is a function that takes one or more values and return a string.  The values do not need to be strings as Clojure will dynamically convert them.

!!! NOTE "Join strings and a numeric value"
    ```clojure
    (str "hello world" " " 2)
    ```

Expected result

```clojure
"hello world 2"
```

!!! NOTE "Join strings and the result of a function call"
    ```clojure
    (str "hello world" " " (+ 1 2))
    ```

Expected result

```clojure
"hello world 3"
```

`+` is the name of a function, its qualified name is `clojure.core/+`.  `+` takes zero or more arguments, adds the values together and returns the result.

A function call always returns a value so can be used as an argument to another funciton, or anywhere a value would be used.


!!! INFO "Implicit types"
    Clojure uses types underneath and infers the type of something by its literal shape

    `"string"` is a string type, `java.lang.String`

    `123` is an Integer value, `java.lang.Long`

    `3.14` is a Decimal value, `java.lang.Double`

    `22/7` is a Ratio value, `clojure.lang.Ratio` (used to preserve accuracy of data)
