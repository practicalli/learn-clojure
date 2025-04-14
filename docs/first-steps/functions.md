# Functions

The Clojure Standard Library contains hundreds of functions for writing custom applications and services.

`defn` function is used to define functions


```clojure
(defn hello-world 
  [name]
  (str "Hello" name ", welcome to Clojure")
```

Evaluate the `hello-world` function definition expression, then call the `hello-world` function with an argument

```clojure
(hello-world "Jenny")
```

The last expression in a function definition is returned when the function is called.  An explicity return expression is not required.

!!! INFO "Function definition must be evaluated before use"
    In a Clojure source code file, the function definition code must be written before any code that calls that function, otherwise a symbol not found error will occur

    During development, experimental code can be written after it is called as long as that definition has been evaluated in the REPL.  e.g. a new design for a function could be written in a rich comment, `(comment ,,,)` at the bottom of the source code file and evaluated to replace a definition further up in the source code file.  If the new design for the function is preferred its code would replace the original definition.  If the new design is interesting but not desired, it could be kept as part of a design journal.


??? CLOJURE-IDIOM "Avoid using declare"
    `declare` allows for a symbol to be defined without a value.  A function name could be defined at the top of a Clojure source code file with the function definition, `defn`, occuring lower down in the file after code that calls the function.

    It is commonly viewed as a need to refactor the design if `declare` is required, e.g. two functions call each other.  Function definitions should be defined so that `declare` is not required, typically by extracting code into other functions.


## Function arguments

Argument names are defined in a vector, `[]`

Functions are limited to recieving 26 arguments. In reality a minimal number should be passed or multiple values passed as a collection.

`& arguments` in the function definition will take zero or more values, placing all values into a vector, `[]`

`:as` to group all arguments as one collection of values  

!!! DESIGN "none, one, or many arguments"
    Practicalli recommends designing a function to recieve either no arguments, one argument or many arguments

    A hash-map can be considered one argument that abstracts many arguments.


!!! CLOJURE-IDIOM "hash-maps as arguments"
    Pass a hash-map as an argument to a function provides scope for growth.

    Keys can be added to the hash-map to extend the capabilities of the function without breaking existing calls to the function.

    Using keys and values provides more context as to the purpose of the arguments passed.

!!! CLOJURE-IDIOM "unnamed arguments"
    `_` is used as an argument name when an argument has to be passed to a function, but the function is not going to use the argument.

    `_` may be used to represent the request hash-map passed to handler functions in Clojure services and APIs.

## Destructure arguments

Associative destructuring is the most common approach to function definition design

`:keys` declaration generates local names from the matching keywords given in a vector, `[]`

```clojure
(defn destructure-arguments
  [{:keys [a b c]}]
  (str "Extracted keys have the values:" a b c)
)
```

## Function specifications

`clojure.spec` can be used to instrument a function, checking the correct types of values are passed as arguments.
