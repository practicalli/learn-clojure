# Code Kata: Combination Lock

An exercise to become more comfortable with the concept of _List Comprehension_


<!--sec data-title="Theory: List Comprehension..." data-id="answer001" data-collapse=true ces-->
Lisp comprehension


In Clojure, list comprehension is via the [for](https://clojuredocs.org/clojure.core/for) function

* Clojure docs: [for](https://clojuredocs.org/clojure.core/for)
* Reference: [Lisp Comprehension in Clojure](https://practicalli.github.io/clojure/thinking-functionally/list-comprehension.html) - Clojure, practicalli
<!--endsec-->


> ####Note::Generate all the combinations for a lock with 3 combination tumblers
> Each combination is made up of three numbers (the tumblers).
> Each number is between 0 and 9
```eval-clojure

```

<!--sec data-title="Hint..." data-id="answer002" data-collapse=true ces-->
The range function can generate a sequence of numbers, so could be used to create each tumbler.

Combinations can be returned as a collection, eg. a vector.
<!--endsec-->


<!--sec data-title="Example Solution: Dont look until you have tried to solve it first..." data-id="answer003" data-collapse=true ces-->
```clojure
(for [tumbler-1 (range 10)
      tumbler-2 (range 10)
      tumbler-3 (range 10)]
 [tumbler-1 tumbler-2 tumbler-3])
```
<!--endsec-->



## Alternative Exercises

Here are some exercises that could also be solved with list comprehension

* Fizzbuzz

