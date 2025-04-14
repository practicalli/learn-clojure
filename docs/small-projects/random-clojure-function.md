# Random Clojure Function

<!-- https://simplewordcloud.com/ used to generate word cloud -->
![Clojure function names word cloud](https://raw.githubusercontent.com/practicalli/graphic-design/live/code-challenges/clojure-function-word-cloud.png){align=right loading=lazy style="height:300px;width:300px"}

A simple application that returns a random function from the `clojure.core` namespace, along with the function argument list and its description (from the doc-string)

There are 659 functions in [`clojure.core` namespace](https://clojuredocs.org/clojure.core) and 955 in the standard library (as of June 2020).  These functions are learned over time as experience is gained with Clojure.

??? EXAMPLE "Project: Random Clojure function"
    [:fontawesome-brands-github: practicalli/random-clojure-function](https://github.com/practicalli/random-clojure-function){target=_blank} repository contains a Clojure project with an example solution

## Live Coding Video walk-through

A [Live coding video walk-through of this project](https://youtu.be/sXZKrD4cAFk) shows how this application was developed, using Spacemacs editor and CircleCI for continuous integration.

??? HINT " -M flag superseeds -A flag"
    The `-M` flag replaced the `-A` flag when running code via `clojure.main`, e.g. when an alias contains `:main-opts`.  The `-A` flag should be used only for the specific case of including an alias when starting the Clojure CLI built-in REPL.

<p style="text-align:center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/sXZKrD4cAFk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

## Create a project

Use the `:project/create` [:fontawesome-solid-book-open: Practicalli Clojure CLI Config](/clojure/clojure-cli/practicalli-config/) to create a new Clojure project.

```bash
clojure -T:project/create :template app :name practicalli/random-function
```

This project has a `deps.edn` file that includes the aliases

- `:test` - includes the `test/` directory in the class path so unit test code is found
- `:runner` to run the Cognitect Labs test runner which will find and run all unit tests

## REPL experiments

Open the project in a Clojure-aware editor or start a Rebel terminal UI REPL

=== "Clojure Editor"
    Open the `src/practicalli/random-function.clj` file in a Clojure aware editor and start a REPL process (jack-in)

    Optionally create a rich comment form that will contain the expressions as the design of the code evolves, moving completed functions out of the comment forms so they can be run by evaluating the whole namespace.
    ```clojure
    (ns practicalli.random-function)

    (comment
      ;; add experimental code here
    )
    ```

=== "Rebel REPL"
    Open a terminal and change to the root of the Clojure project created, i.e. the directory that contains the `deps.edn` file.

    Start a REPL that provides a rich terminal UI
    ```shell
    clojure -M:repl/reloaded
    ```
    `require` will make a namespace available from within the REPL
    ```clojure
    (require '[practicalli.random-function])
    ```
    Change into the `random-function` namespace to define functions
    ```clojure
    (in-ns 'practicalli.random-function')
    ```
    Reload changes made to the `src/practicalli/random_function.clj` file using the `require` function with the `:reload` option.  `:reload` forces the loading of all the definitions in the namespace file, overriding any functions of the same name in the REPL.
    ```clojure
    (require '[practicalli.random-function] :reload)
    ```

    !!! HINT "Copy finished code into the source code files"
        Assuming the code should be kept after the REPL is closed, save the finished versions of function definitions into the source code files.  Use ++arrow-up++ and ++arrow-down++ keys at the REPL prompt to navigate the history of expressions

List all the public functions in the `clojure.core` namespace using the `ns-publics` function

```clojure
(ns-publics 'clojure.core)
```

The hash-map keys are function symbols and the values are the function vars

```clojure
{+' #'clojure.core/+',
 decimal? #'clojure.core/decimal?,
 sort-by #'clojure.core/sort-by,
 macroexpand #'clojure.core/macroexpand
 ,,,}
```

The `meta` function will return a hash-map of details about a function, when given a function var.

```clojure
(meta #'map)
```

The hash-map has several useful pieces of information for the application, including `:name`, `:doc`, and `:arglists`

```clojure
;; => {:added "1.0",
;;     :ns #namespace[clojure.core],
;;     :name map,
;;     :file "clojure/core.clj",
;;     :static true,
;;     :column 1,
;;     :line 2727,
;;     :arglists ([f] [f coll] [f c1 c2] [f c1 c2 c3] [f c1 c2 c3 & colls]),
;;     :doc
;;     "Returns a lazy sequence consisting of the result of applying f to\n  the set of first items of each coll, followed by applying f to the\n  set of second items in each coll, until any one of the colls is\n  exhausted.  Any remaining items in other colls are ignored. Function\n  f should accept number-of-colls arguments. Returns a transducer when\n  no collection is provided."}
```

To use the `meta` function, the values from the `ns-publics` results should be used.

```clojure
(vals (ns-publics 'clojure.core))
```

`rand-nth` will return a random function var from the sequence of function vars

```clojure
(rand-nth (vals (ns-publics 'clojure.core)))
```

A single function var is returned, so then the specific meta data can be returned.

```clojure
(meta (rand-nth (vals (ns-publics 'clojure.core))))
```

## Define a name for all functions

Edit the `src/practicalli/random-function.clj` file and define a name for the collection of all public functions from `clojure.core`

```clojure
(def standard-library
  "Fully qualified function names from clojure.core"
  (vals (ns-publics 'clojure.core)))
```

## Write Unit Tests

From the REPL experiments we have a basic approach for the application design, so codify that design by writing unit tests.  This will also highlight regressions during the course of development.

Edit the file `test/practicalli/random_function_test.clj` and add unit tests.

The first test check the standard-library-functions contains entries.

The second test checks the -main function returns a string (the function name and details).

```clojure title="src/practicalli/random_function_test.clj"
(ns practicalli.random-function-test
  (:require [clojure.test :refer [deftest is testing]]
            [practicalli.random-function :as random-fn]))

(deftest standard-library-test
  (testing "Show random function from Clojure standard library"
    (is (seq random-fn/standard-library-functions))
    (is (string? (random-fn/-main)))))
```

## Update the main function

Edit the `src/practicalli/random-function.clj` file.  Change the `-main` function to return a string of the function name and description.

```clojure title="src/practicalli/random-function.clj"
(defn -main
  "Return a function name from the Clojure Standard library"
  [& args]
  (let [function-details (meta (rand-nth standard-library-functions))]
    (str (function-details :name) "\n  " (function-details :doc)))
  )
```

=== "Cognitect Test Runner"
    Run the tests with the Congnitect test runner via the `test` function in the `build.clj` file.
    ```bash
    clojure -T:build test

```

=== "Kaocha Test Runner"
    Run the tests with the Kaocha test runner using the alias `:test/run` from [Practicalli Clojure CLI config](/clojure/clojure-cli/practialli-config/)
    ```bash
    clojure -M:test/run
```

## Running the application

Use the clojure command with the main namespace of the application.  Clojure will look for the -main function and evaluate it.

```bash
clojure -M -m practicalli.random-function
```

This should return a random function name and its description.  However, nothing is returned.  Time to refactor the code.

## Improving the code

The tests pass, however, no output is shown when the application is run.

The main function returns a string but nothing is sent to standard out, so running the application does not return anything.

The `str` expression could be wrapped in a println, although that would make the result harder to test and not be very clean code.  Refactor the `-main` to a specific function seems appropriate.

Replace the `-main-test` with a `random-function-test` that will be used to test a new function of the same name which will be used for retrieving the random Clojure function.

```clojure
(deftest random-function-test
  (testing "Show random function from Clojure standard library"
    (is (seq SUT/standard-library-functions))
    (is (string? (SUT/random-function SUT/standard-library-functions)))))
```

Create a new function to return a random function from a given collection of functions, essentially moving the code from `-main`.

The function extracts the function `:name` and `:doc` from the metadata of the randomly chosen function.

```clojure
(defn random-function
  [function-list]
  (let [function-details (meta (rand-nth function-list))]
    (str (function-details :name) "\n  " (function-details :doc) "\n  ")))
```

Update the main function to call this new function.

```clojure
(defn -main
  "Return a function name from the Clojure Standard library"
  [& args]
  (println (random-function standard-library-functions)))
```

Run the tests again.

If the tests pass, then run the application again

```bash
 clojure -M -m practicalli.random-function
```

A random function and its description are displayed.

## Adding the function signature

Edit the `random-function` code and add the function signature to the string returned by the application.

Format the code so it is in the same structure of the output it produces, making the code clearer to understand.

```clojure
(defn random-function
  [function-list]
  (let [function-details (meta (rand-nth function-list))]
    (str (function-details :name)
    "\n  " (function-details :doc)
    "\n  " (function-details :arglists))))
```

## Add more namespaces

All current namespaces on the classpath can be retrieved using the `all-ns` function.  This returns a lazy-seq, `(type (all-ns))`

```clojure
(all-ns)
```

Using the list of namespace the `ns-publics` can retrieve all functions across all namespaces.

Create a helper function to get the functions from a namespace, as this is going to be used in several places.

```clojure
(defn function-list
  [namespace]
  (vals (ns-publics namespace)))
```

This function can be mapped over all the namespaces to get a sequence of all function vars.  Using `map` creates a sequence for each namespace, returned as a sequence of sequences.  Using `mapcat` will concatenate the nested sequences and return a flat sequence of function vars.

```clojure
(mapcat #(vals (ns-publics %)) (all-ns))
```

Bind the results of this expression to the name `all-public-functions`.

```clojure
(def available-namespaces
  (mapcat #(vals (ns-publics %)) (all-ns)))
```

## Control which namespaces are consulted

There is no way to control which library we get the functions from, limiting the ability of our application.

Refactor the main namespace to act differently based on arguments passed:

1. If no arguments are passed then all public functions are used to pull a random function from.

2. If any argument is passed, the argument should be used as the namespace to pull a random function from.  The argument is assumed to be a string.

`ns-publics` function needs a namespace as a symbol, so the `symbol` function is used to convert the argument.

```clojure
(symbol "clojure.string")
```

The `-main` function uses `[& args]` as a means to take multiple arguments. All arguments are put into a vector, so the symbol function should be mapped over the elements in the vector to create symbols for all the namespaces.

Use an anonymous function to convert the arguments to symbols and retrieve the list of public functions from each namespace.  This saves mapping over the arguments twice.

`mapcat` the function-list function over all the namespaces, converting each namespace to a symbol.

```clojure
(mapcat #(function-list (symbol %)) args)
```

Update the main function with an if statement.  Use `seq` as the condition to test if a sequence (the argument vector) has any elements (namespaces to use).

If there are arguments, then get the functions for the specific namespaces.

Else return all the functions from all the namespaces.

```clojure
(defn -main
  "Return a function name from the Clojure Standard library"
  [& args]
  (if (seq args)
    (println (random-function (mapcat #(function-list (symbol %)) args)))
    (println (random-function standard-library-functions))))
```

## Use the fully qualified name for the namespace

Now that functions can come from a range of namespaces, the fully qualified namespace should be used for the function, eg. domain/namespace

```clojure
(:ns (meta (rand-nth standard-library-functions)))
```

Update the random function to return the domain part of the namespace, separated by a `/`

```clojure
(defn random-function
  [function-list]
  (let [function-details (meta (rand-nth function-list))]
    (str (function-details :ns) "/" (function-details :name)
         "\n  " (function-details :arglists)
         "\n  " (function-details :doc))))
```

## Use all available namespaces by default

Define a name to represent the collection of all available namespaces, in the context of the running REPL.

```clojure
(def all-public-functions
  "Fully qualified function names from available"
  (mapcat #(vals (ns-publics %)) (all-ns)))
```

Update the `-main` function to use all available namespaces if no arguments are passed to the main function.

```clojure
(defn -main
  "Return a random function and its details
  from the available namespaces"
  [& args]
  (if (seq args)
    (println (random-function (mapcat #(function-list (symbol %)) args)))
    (println (random-function all-public-functions))))
```

## Follow-on idea: Convert to a web service

Add http-kit server and send  information back as a plain text, html, json and edn

## Follow-on idea: Convert to a library

Convert the project to a library so this feature can be used as a development tool for any project.

Add functionality to list all functions from all namespaces or a specific namespace, or functions from all namespaces of a particular domain, e.g `practicalli` or `practicalli.app`

<!-- > #### Hint::Evaluating namespaces on REPL start -->
<!-- > The REPL does not evaluate project code on start-up.  If it did and that code had a error, it may prevent the REPL from starting. -->
<!-- > -->
<!-- > Standard practice is to required the main namespace for the project, then switch the REPL to that namespace.  The functions for the project are now available. -->
<!-- > To require and switch to a namespace on startup, use the `clojure` or `clj` commands with the --eval option to run the specific commands.  The --repl option will ensure the repl starts. -->
<!-- ```bash -->
<!-- clj --eval "(require 'practicalli.random-clojure-core-function)" --eval "(in-ns 'practicalli.random-clojure-core-function)" --repl -->
<!-- ``` -->
<!-- > The --eval approach will be blocked if used with aliases that set the main namespace, such as `:rebel`. -->
