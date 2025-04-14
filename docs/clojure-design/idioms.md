# Clojure Design Idioms

An idiom is the Clojure way

Idioms are not Design Patterns found in languages like C++ and Java.  Design patterns have a very specific language and structure and carefully design the context in which they are used.

Idioms are very simple in comparision, they are the way Clojure should be used.


Idiomatic Clojure is a term commonly used, commonly as a question.  Is this idiomatic Clojure?


## Idioms from style guide

A (WIP) refactor of the [idioms from the Clojure Style guide](https://guide.clojure.style/#idioms)


### Dynamic Namespace Manipulation

TODO: vauge statement with no example

Avoid the use of namespace-manipulating functions like `require` and
`refer`. They are entirely unnecessary outside of a REPL
environment.


### Forward References

TODO: vauge statement with no example

Avoid forward references.  They are occasionally necessary, but such occasions
are rare in practice.

### Declare

Use `declare` to enable forward references when forward references are
necessary.

### Higher-order Functions

TODO: expand and provide examples

Prefer higher-order functions like `map` to `loop/recur`.


## Appropriate Function use

- let def
- if when cond condp 
- Nil Punning (a pun that null is not a value, when it is??) seq empty?



### Use let for local names

`let` should be used inside a function definition or to create a local name when experimenting in the REPL.

`def` defines a symbol name at the scope of the current namespace and therefore is inappropriate to use within a function definition.


!!! WARNING "Avoid vars inside functions"
    ```clojure
    (defn iterative-function 
      "Avoid breaking the scope of data in a function by using def"
      [args]
        (def x 5)  ; Avoid using a var as it breaks the scope of the function
      ...)

    x ; evaluating will return the result without going through the function call and could be used anywere the namespace is required, increasing complexity.
    ```


!!! WARNING "Problematic debuging technique"
    An obscure debug technique is to define a def inside a function definition to capture intermediate values, however, this breaks the scope of Clojure and can lead to incorrect use of Clojure (especially by those new to the language).

    Use a debug tool rather than break the scope of Clojure with a def, e.g. Cider Debug or Flowstorm-debugger.


### Shadowing `clojure.core` Names [[dont-shadow-clojure-core]]

Don't shadow `clojure.core` names with local bindings or custom function definitions (confusion, a change of well held expectations).

```clojure
;; bad - clojure.core/map must be fully qualified inside the function
(defn inc [map]
  ...)
```

RELATED: avoid renaming functions required by name via the `ns` definition. 


### Alter Var Binding [[alter-var]]

Use `alter-var-root` instead of `def` to change the value of a var.

[source,clojure]
```
;; good
(def thing 1) ; value of thing is now 1
; do some stuff with thing
(alter-var-root #'thing (constantly nil)) ; value of thing is now nil

;; bad
(def thing 1)
; do some stuff with thing
(def thing nil)
; value of thing is now nil
```

### Nil Punning [[nil-punning]]

Use `seq` as a terminating condition to test whether a sequence is
empty (this technique is sometimes called _nil punning_).

[source,clojure]
```
;; good
(defn print-seq [s]
  (when (seq s)
    (prn (first s))
    (recur (rest s))))

;; bad
(defn print-seq [s]
  (when-not (empty? s)
    (prn (first s))
    (recur (rest s))))
```

### Converting Sequences to Vectors [[to-vector]]

Prefer `vec` over `into` when you need to convert a sequence into a vector.

[source,clojure]
```
;; good
(vec some-seq)

;; bad
(into [] some-seq)
```

### Converting Something to Boolean

Use the `boolean` function if you need to convert something to an actual boolean value (`true` or `false`).

[source,clojure]
```
;; good
(boolean (foo bar))

;; bad
(if (foo bar) true false)
```

NOTE: Don't forget that the only values in Clojure that are "falsey" are `false` and `nil`. Everything else
will evaluate to `true` when passed to the `boolean` function.

You'll rarely need an actual boolean value in Clojure, but it's useful to know how to obtain one when you do.

### `when` vs `if` [[when-instead-of-single-branch-if]]

Use `when` instead of `if` with just the truthy branch, as in `(if condition (something...))` or `(if ... (do ...))`.

[source,clojure]
```
;; good
(when pred
  (foo)
  (bar))

;; bad
(if pred
  (do
    (foo)
    (bar)))
```

### `if-let` [[if-let]]

Use `if-let` instead of `let` + `if`.

[source,clojure]
```
;; good
(if-let [result (foo x)]
  (something-with result)
  (something-else))

;; bad
(let [result (foo x)]
  (if result
    (something-with result)
    (something-else)))
```

### `when-let` [[when-let]]

Use `when-let` instead of `let` + `when`.

[source,clojure]
```
;; good
(when-let [result (foo x)]
  (do-something-with result)
  (do-something-more-with result))

;; bad
(let [result (foo x)]
  (when result
    (do-something-with result)
    (do-something-more-with result)))
```

### `if-not` [[if-not]]

Use `if-not` instead of `(if (not ...) ...)`.

[source,clojure]
```
;; good
(if-not pred
  (foo))

;; bad
(if (not pred)
  (foo))
```

### `when-not` [[when-not]]

Use `when-not` instead of `(when (not ...) ...)`.

[source,clojure]
```
;; good
(when-not pred
  (foo)
  (bar))

;; bad
(when (not pred)
  (foo)
  (bar))
```

### `when-not` vs `if-not` [[when-not-instead-of-single-branch-if-not]]

Use `when-not` instead of `(if-not ... (do ...))`.

[source,clojure]
```
;; good
(when-not pred
  (foo)
  (bar))

;; bad
(if-not pred
  (do
    (foo)
    (bar)))
```

### `not=` [[not-equal]]

Use `not=` instead of `(not (= ...))`.

[source,clojure]
```
;; good
(not= foo bar)

;; bad
(not (= foo bar))
```

### `printf` [[printf]]

Prefer `printf` over `(print (format ...))`.

[source,clojure]
```
;; good
(printf "Hello, %s!\n" name)

;; ok
(println (format "Hello, %s!" name))
```

### Flexible Comparison Functions

When doing comparisons, leverage the fact that Clojure's functions `<`,
`>`, etc. accept a variable number of arguments.

[source,clojure]
```
;; good
(< 5 x 10)

;; bad
(and (> x 5) (< x 10))
```

### Single Parameter Function Literal [[single-param-fn-literal]]

Prefer `%` over `%1` in function literals with only one parameter.

[source,clojure]
```
;; good
#(Math/round %)

;; bad
#(Math/round %1)
```

### Multiple Parameters Function Literal [[multiple-params-fn-literal]]

Prefer `%1` over `%` in function literals with more than one parameter.

[source,clojure]
```
;; good
#(Math/pow %1 %2)

;; bad
#(Math/pow % %2)
```

### No Useless Anonymous Functions [[no-useless-anonymous-fns]]

Don't wrap functions in anonymous functions when you don't need to.

[source,clojure]
```
;; good
(filter even? (range 1 10))

;; bad
(filter #(even? %) (range 1 10))
```

### No Multiple Forms in Function Literals [[no-multiple-forms-fn-literals]]

Don't use function literals if the function body will consist of
more than one form.

[source,clojure]
```
;; good
(fn [x]
  (println x)
  (* x 2))

;; bad (you need an explicit do form)
#(do (println %)
     (* % 2))
```

### Anonymous Functions vs `complement`, `comp` and `partial`

Prefer anonymous functions over `complement`, `comp` and `partial`, as this results
in simpler code most of the time.footnote:[You can read more on the subject https://ask.clojure.org/index.php/8373/when-should-prefer-comp-and-partial-to-anonymous-functions[here].]

###= `complement` [[complement]]

[source,clojure]
```
;; good
(filter #(not (some-pred? %)) coll)

;; okish
(filter (complement some-pred?) coll)
```

###= `comp` [[comp]]

[source,clojure]
```
;; Assuming `(:require [clojure.string :as str])`...

;; good
(map #(str/capitalize (str/trim %)) ["top " " test "])

;; okish
(map (comp str/capitalize str/trim) ["top " " test "])
```

`comp` is quite useful when composing transducer chains, though.

[source,clojure]
```
;; good
(def xf
  (comp
    (filter odd?)
    (map inc)
    (take 5)))
```

###= `partial` [[partial]]

[source,clojure]
```
;; good
(map #(+ 5 %) (range 1 10))

;; okish
(map (partial + 5) (range 1 10))
```

### Threading Macros [[threading-macros]]

Prefer the use of the threading macros `+->+` (thread-first) and `+->>+`
(thread-last) to heavy form nesting.

[source,clojure]
```
;; good
(-> [1 2 3]
    reverse
    (conj 4)
    prn)

;; not as good
(prn (conj (reverse [1 2 3])
           4))

;; good
(->> (range 1 10)
     (filter even?)
     (map (partial * 2)))

;; not as good
(map (partial * 2)
     (filter even? (range 1 10)))
```

### Threading Macros and Optional Parentheses

Parentheses are not required when using the threading macros for functions having no argument specified, so use them only when necessary.

[source,clojure]
```
;; good
(-> x fizz :foo first frob)

;; bad; parens add clutter and are not needed
(-> x (fizz) (:foo) (first) (frob))

;; good, parens are necessary with an arg
(-> x
    (fizz a b)
    :foo
    first
    (frob x y))
```

### Threading Macros Alignment

The arguments to the threading macros `+->+` (thread-first) and `+->>+`
(thread-last) should line up.

[source,clojure]
```
;; good
(->> (range)
     (filter even?)
     (take 5))

;; bad
(->> (range)
  (filter even?)
  (take 5))
```

### Default `cond` Branch [[else-keyword-in-cond]]

Use `:else` as the catch-all test expression in `cond`.

[source,clojure]
```
;; good
(cond
  (neg? n) "negative"
  (pos? n) "positive"
  :else "zero")

;; bad
(cond
  (neg? n) "negative"
  (pos? n) "positive"
  true "zero")
```

### `condp` vs `cond` [[condp]]

Prefer `condp` instead of `cond` when the predicate & expression don't
change.

[source,clojure]
```
;; good
(cond
  (= x 10) :ten
  (= x 20) :twenty
  (= x 30) :thirty
  :else :dunno)

;; much better
(condp = x
  10 :ten
  20 :twenty
  30 :thirty
  :dunno)
```

### `case` vs `cond/condp` [[case]]

Prefer `case` instead of `cond` or `condp` when test expressions are
compile-time constants.

[source,clojure]
```
;; good
(cond
  (= x 10) :ten
  (= x 20) :twenty
  (= x 30) :forty
  :else :dunno)

;; better
(condp = x
  10 :ten
  20 :twenty
  30 :forty
  :dunno)

;; best
(case x
  10 :ten
  20 :twenty
  30 :forty
  :dunno)
```

### Short Forms In Cond [[short-forms-in-cond]]

Use short forms in `cond` and related.  If not possible give visual
hints for the pairwise grouping with comments or empty lines.

[source,clojure]
```
;; good
(cond
  (test1) (action1)
  (test2) (action2)
  :else   (default-action))

;; ok-ish
(cond
  ;; test case 1
  (test1)
  (long-function-name-which-requires-a-new-line
    (complicated-sub-form
      (-> 'which-spans multiple-lines)))

  ;; test case 2
  (test2)
  (another-very-long-function-name
    (yet-another-sub-form
      (-> 'which-spans multiple-lines)))

  :else
  (the-fall-through-default-case
    (which-also-spans 'multiple
                      'lines)))
```

### Set As Predicate [[set-as-predicate]]

Use a `set` as a predicate when appropriate.

[source,clojure]
```
;; good
(remove #{1} [0 1 2 3 4 5])

;; bad
(remove #(= % 1) [0 1 2 3 4 5])

;; good
(count (filter #{\a \e \i \o \u} "mary had a little lamb"))

;; bad
(count (filter #(or (= % \a)
                    (= % \e)
                    (= % \i)
                    (= % \o)
                    (= % \u))
               "mary had a little lamb"))
```

### `inc` and `dec` [[inc-and-dec]]

Use `(inc x)` & `(dec x)` instead of `(+ x 1)` and `(- x 1)`.

### `pos?` and `neg?` [[pos-and-neg]]

Use `(pos? x)`, `(neg? x)` & `(zero? x)` instead of `(> x 0)`,
`(< x 0)` & `(= x 0)`.

### `list*` vs `cons` [[list-star-instead-of-nested-cons]]

Use `list*` instead of a series of nested `cons` invocations.

[source,clojure]
```
;; good
(list* 1 2 3 [4 5])

;; bad
(cons 1 (cons 2 (cons 3 [4 5])))
```

### Sugared Java Interop [[sugared-java-interop]]

Use the sugared Java interop forms.

```clojure
;;; object creation
;; good
(java.util.ArrayList. 100)

;; bad
(new java.util.ArrayList 100)

;;; static method invocation
;; good
(Math/pow 2 10)

;; bad
(. Math pow 2 10)

;;; instance method invocation
;; good
(.substring "hello" 1 3)

;; bad
(. "hello" substring 1 3)

;;; static field access
;; good
Integer/MAX_VALUE

;; bad
(. Integer MAX_VALUE)

;;; instance field access
;; good
(.someField some-object)

;; bad
(. some-object someField)
```

### Compact Metadata Notation For True Flags [[compact-metadata-notation-for-true-flags]]

Use the compact metadata notation for metadata that contains only
slots whose keys are keywords and whose value is boolean `true`.

[source,clojure]
```
;; good
(def ^:private a 5)

;; bad
(def ^{:private true} a 5)
```

### Private

Denote private parts of your code.

[source,clojure]
```
;; good
(defn- private-fun [] ...)

(def ^:private private-var ...)

;; bad
(defn private-fun [] ...) ; not private at all

(defn ^:private private-fun [] ...) ; overly verbose

(def private-var ...) ; not private at all
```

### Access Private Var [[access-private-var]]

To access a private var (e.g. for testing), use the `@#'some.ns/var` form.

### Attach Metadata Carefully [[attach-metadata-carefully]]

Be careful regarding what exactly you attach metadata to.

[source,clojure]
```
;; we attach the metadata to the var referenced by `a`
(def ^:private a {})
(meta a) ;=> nil
(meta #'a) ;=> {:private true}

;; we attach the metadata to the empty hash-map value
(def a ^:private {})
(meta a) ;=> {:private true}
(meta #'a) ;=> nil
```

