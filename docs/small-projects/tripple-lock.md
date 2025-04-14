# Triple Lock

A new safe too keep all the richest you will gain from becoming a Clojure developer (hopefully).  The safe has a 3 combination lock to protect your new found wealth, but just how safe is the safe?

## Create a new Clojure project

[:fontawesome-solid-book-open: Pracitcalli Clojure CLI Config](/clojure/clojure-cli/practicalli-config/) provides the `:project/create` alias to create projects using deps-new project.

```bash
clojure -T:project/create :name practicalli/triple-lock
```

## Designing the combination lock

Lets consider how we would create such a combination lock in Clojure.

- The combination is managed by three tumbler wheels
- Each tumbler wheel has the same range of numbers on then, 0 to 9

Each tumbler wheel could have all the numbers it contains within a _Collection_ in Clojure.  The simplest approach would be to put the numbers 0 to 9 into a Vector (an array-like collection).

```clojure
[0 1 2 3 4 5 6 7 8 9]
```

As the numbers on the tumbler wheel are just a range between 0 and 9, then rather than type out all the numbers we can use the `range` function to generate all the numbers for us.

When we give the range function one argument, it will create all the whole numbers from 0 to the number before that of the argument.  In the following example, we give `range` the argument of 10 and we receive the numbers from 0 to 9.

```clojure
(range 10)
```

`range` can take two arguments to specify the start and end points, e.g `(range 5 15)` which returns `(5 6 7 8 9 10 11 12 13 14)` 

`range` can also include a step increment to, e.g. `(range 2 0 20)` which returns every second number from the range, ``

??? HINT "range without arguments generates infinite sequence"
    Avoid callng the `range` function without arguments as it will try and generate an infinite range of numbers, not stopping until the computer memory is used up.

    `range` can be used without arguments safely when another function limits the numbers generated, e.g. `take`

    ```clojure
    (take 10 (range))
    ```

## Create all the Combinations

Complete the following code (replacing the `,,,`) to generate all the possible combinations of the lock**

```clojure
(for [tumbler-1 (range 10)
      ,,,     ,,,
      ,,,     ,,,]
 [tumbler-1 ,,,   ,,,])
```

## Count combinations

Instead of showing all the possible combinations, count all the combinations and return the total number of combinations

Take the code from the combinations and wrap it in the `count` function

```clojure
;; now count the possible combinations
(count

         )
```

## Unique combinations

Make the lock more secure by only allowing combinations where each tumbler wheel has a different number, avoiding easier to guess combinations like 1-1-1, 1-2-2, 1-2-1, etc.

How many combinations does that give us?

Complete the following code to create a 3-tumbler wheel combination lock, where none of the numbers are the same**

> Hint: Beware not to enter (range) without an argument as Clojure may try and evaluate infinity

```clojure
(count (for [tumbler-1 (range 10)
             ,,,     ,,,
             ,,,     ,,,
             :when (or (= tumbler-1 tumbler-2)
                       ,,,
                       ,,,)]
         [tumbler-1 ,,,   ,,,]))
```

??? EXAMPLE "Suggested solution"
    Suggested solution to the completed 3-lock challenges.
    ```clojure
    ;; a 3 combination padlock

    ;; model the combinations
    (for [tumbler-1 (range 10)
          tumbler-2 (range 10)
          tumbler-3 (range 10)]
     [tumbler-1 tumbler-2 tumbler-3])


    ;; now count the possible combinations
    (count (for [tumbler-1 (range 10)
                 tumbler-2 (range 10)
                 tumbler-3 (range 10)]
             [tumbler-1 tumbler-2 tumbler-3]))


    (count (for [tumbler-1 (range 10)
                 tumbler-2 (range 10)
                 tumbler-3 (range 10)
                 :when (or (= tumbler-1 tumbler-2)
                           (= tumbler-2 tumbler-3)
                           (= tumbler-3 tumbler-1))]
             [tumbler-1 tumbler-2 tumbler-3]))

    ;; lets look at the combinations again, we can see that there is always at least 2 matching values.  This is probably the opposite of what we want in real life.
    (for [tumbler-1 (range 10)
          tumbler-2 (range 10)
          tumbler-3 (range 10)
          :when (or (= tumbler-1 tumbler-2)
                    (= tumbler-2 tumbler-3)
                    (= tumbler-3 tumbler-1))]
      [tumbler-1 tumbler-2 tumbler-3])
    ```
