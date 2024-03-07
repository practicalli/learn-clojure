# Clojure Quick Reference

The basic Clojure syntax and a few common functions you should probably learn first.

The examples are editable (using an embedded REPL) so feel free to experiment and watch as the return value changes as you change the code.  Reload the page if you want to reset all the code back to the starting point.

[Install Clojure](/clojure-cli/install/) on your computer if you want to experiment even further.

!!! HINT "Want to go deeper already?"
    Watch [the Clojure language video series by Brian Will](https://www.youtube.com/playlist?list=PLAC43CFB134E85266) for a detailed introduction to key parts of the language.  Or discover Clojure core functions by completing challenges on [4Clojure.org](https://4clojure.oxal.org/) and then [watching how Practicalli solved them](https://www.youtube.com/playlist?list=PLpr9V-R8ZxiDB_KGrbliCsCUrmcBvdW16).

## Calling functions

The first element in a list, `()`, is a call to a function.  Any other elements are passed to the function as arguments. The examples show how to call functions with multiple arguments.

!!! EXAMPLE "Add values together"
    ```clojure
    (+ 1 2 3 4 5)
    ```

!!! EXAMPLE "Parens rather than precedence order rules"
    ```clojure
    (+ 3 (* 2 (- 7 2) 4) (/ 16 4))
    ```

!!! EXAMPLE "Construct a string from strings and return value of a function"
    ```clojure
    (str "Clojure is " (- 2021 2007) " years old")
    ```

!!! EXAMPLE "Increment a value"
    ```clojure
    (inc 1)
    ```

!!! EXAMPLE "Increment all values in a collection"
    ```clojure
    (map inc [1 2 3 4 5])
    ```

!!! EXAMPLE "Return odd values from a sequence of values"
    ```clojure
    (filter odd? (range 11))
    ```

!!! Hint "Prefix notation and parens"
    Hugging code with `()` is a simple syntax to define the scope of code expressions.  No additional `;`, `,` or spaces are required.

    Treating the first element of a list as a function call is referred to as prefix notation, which greatly simplifies Clojure syntax.  Prefix notation makes mathematical expressions completely deterministic, eliminating the need for [operator precedence](https://en.wikipedia.org/wiki/Order_of_operations).

## Understanding functions

`clojure.repl/doc` function returns the doc-string of the given function. A doc-string should be part of all public function definitions.

Clojure editors should provide commands to view doc-strings and the ability to jump to function definitions to view their source code

!!! EXAMPLE "Show the documentation for a function"
    ```clojure
    (clojure.repl/doc map)
    ```

## Modeling data with Collection types

Clojure has 4 main collection types, all immutable (cannot change once created) and can contain any Clojure types.

A list, `()`, used for calling functions and representing sequences. A linked list for sequential access.

!!! EXAMPLE "Compose a string from strings an a function return value"
    ```clojure
    (str "lists used mainly " (* 2 2) " " :code)
    ```

A vector, `[]`, used for simple collections of values.  An indexed data structure for random access

!!! EXAMPLE "A vector with different types of values"
    ```clojure
    [0 "indexed" :array (* 2 2) "random-access" 4 :data]
    ```

A map, `{}`, use for descriptive data collections.  An associative data structure for value lookup by unique keys (also known as a dictionary).

!!! EXAMPLE "A hash-map - key value pairs"
    ```clojure
    {:hash-map :associative-collection :pairs {:key "value"} :aka "dictionary"}
    ```

A set, `#{}`, contains a unique set of values. Sets can be used to test if a value is contained within a collection

!!! EXAMPLE "A Set of unique values"
    ```clojure
    #{1 2 3 4 "unique" "set" "of" "values" "unordered" (* 3 9)}
    ```

!!! Hint "Collections are Persistent data types"
    List, Vector, Hash-map and Set are immutable values and cannot be changed.

    When a function transforms the contents of a collection, a new structure is returned which shares all the unchanged values from the original collection.

     The sharing approach is called [persistent data types](https://clojure.org/reference/data_structures#Collections) and enables immutable data to be used efficiently.


## Using data structures

Using the `map` and `inc` function, increment all the numbers in a vector

!!! EXAMPLE "Increment all values in a collection"
    ```clojure
    (map inc [1 2 3 4 5])
    ```

The above `map` function is roughly equivalent to the following expression

!!! EXAMPLE "Conjoin all the values together into a sequence"
    ```clojure
    (conj [] (inc 1) (inc 2) (inc 3) (inc 4) (inc 5))
    ```

The `conj` function creates a new sequenct by combining one or more values.

`map` `reduce` `filter` are common functions for iterating through a collection / sequence of values

!!! EXAMPLE "Multiple a value from each collection in turn"
    ```clojure
    (map * [1 3 5 8 13 21] [3 5 8 13 21 34])
    ```

!!! EXAMPLE "Return all the even values from a collection"
    ```clojure
    (filter even? [1 3 5 8 13 21 34])
    ```

!!! EXAMPLE "Add all the values together in the collection"
    ```clojure
    (reduce + [31 28 30 31 30 31])
    ```

!!! EXAMPLE "Check if a collection contains values"
    ```clojure
    (empty? [])
    ```

!!! HINT "Many Clojure core functions for collections"
    `map`, `reduce`, `apply`, `filter`, `remove` are just a few examples of Clojure core functions that work with data structures.

## Defining custom functions

!!! EXAMPLE "Define a function to calculate the square of a given number"
    ```clojure
    (defn square-of
      "Calculates the square of a given number"
      [number]
      (* number number))

    (square-of 9)
    ```

Function definitions can also be used within other expressions, useful for mapping custom functions over a collection

!!! EXAMPLE "Map an anonymous function over a collection"
    ```clojure
    (map (fn [number] (* number number)) [1 2 3 4 5])
    ```

## Defining local names

Use the `let` function as a simple way to experiment with code designs

!!! EXAMPLE "Create local binding to hold the result of each function"
    ```clojure
    (let [data (range 24 188)
          total (reduce + data)
          values (count data)]
      (str "Average value: " (/ total values)))
    ```

Define local names to remove duplication in function definitions, or to simplify algorithms

!!! EXAMPLE "Define a function and call with a value"
    ```clojure
    (defn square-of
      "Calculates the square of a given number"
      [number]
      (* number number))

    (square-of 9)
    ```

## Defining names for values (vars)

A name bound to a value can be used to represent that value throughout the code.  Names can be bound to simple values (numbers, strings, etc.), collections or even function calls.

`def` binds a name to a value with the scope of the current namespace.  `def` is useful for data that is passed to multiple functions within a namespace.

Evaluating a name will return the value it is bound to.

!!! EXAMPLE "Define a data structure to be shared within the namespace"
    ```clojure
    (def public-health-data
      [{:date "2020-01-01" :confirmed-cases 23814 :recovery-percent 15}
       {:date "2020-01-02" :confirmed-cases 24329 :recovery-percent 14}
       {:date "2020-01-03" :confirmed-cases 25057 :recovery-percent 12}])

    public-health-data
    ```

!!! HINT "def for shared values, let for locally scoped values"
    `let` function is used to bind names to values locally, such as within a function definition.  Names bound with `def` have namespace scope so can be used with any code in that namespace.

## Iterating over collections

`map` iterates a function over a collection of values, returning a new collection of values

!!! EXAMPLE "Pass each argument of the sequence to the function"
    ```clojure
    (map inc (range 20))
    ```

`reduce` iterates a function over the values of a collection to produce a new result

!!! EXAMPLE "Add all values together in the sequence"
    ```
    (reduce + (range 101))
    ```

Reducing functions are function definitions used by the `reduce` function over a collection

!!! EXAMPLE "Use a reducing function over a collection, with an initial start value"
    ```clojure
    (reduce (fn [[numerator denominator] accumulator]
              [(+ numerator accumulator) (inc denominator)])
            [0 0]
            (range 1 20))
    ```

Functions can call themselves to iterate over a collection.  Using a lazy sequence means only the required numbers are generated, ensuring efficiency of operation and making the function usable in many different scenarios.

!!! EXAMPLE "Define a function to calculate a fibonacci sequence"
    ```clojure
    (defn fibonacci-sequence
      [current-number next-number]
      (lazy-seq
        (cons current-number
              (fibonacci-sequence next-number (+ current-number next-number)))))

    (take 10 (fibonacci-sequence 0 1))
    ```

## Host Interoperability

The REPL in this web page is running inside a JavaScript engine, so JavaScript functions can be used from within ClojureScript code 

Clojure uses the Java Virtual Machine as its host, so can call all the classes and methods from the Java language.  Libraries can also be imported from any JVM language, e.g. Java, Groovy, Jython, Jruby, etc.

!!! EXAMPLE "Java Host interop - current date"
    ```clojure
    (java.time.LocalDate/now)
    ```

ClojureScript is the Clojure language that runs in JavaScript environments, e.g. a browser JavaScript engine, node.js

!!! EXAMPLE "JavaScript Host interop - browser alert popup"
    ```clojure
    (js/alert "I am a pop-up alert")
    ```

!!! HINT "Java libraries in Clojure"
    [java.lang library](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/package-summary.html) is available in Clojure by default


## Next steps

[Install Clojure](https://practical.li/clojure/install/) on your computer if you want to experiment even further or keep on reading more about Clojure.

<!-- ## Recursion -->

<!-- Recursive function -->
<!-- ```clojure -->
<!-- (defn recursive-counter -->
<!--   [value] -->
<!--   (if (< value 1000) -->
<!--     (recur (+ value 25)))) -->

<!-- (recursive-counter 100) -->

<!-- ``` -->

<!-- * TODO: reduce and reducing function -->
