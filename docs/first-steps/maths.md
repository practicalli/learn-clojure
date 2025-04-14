# Numbers and Mathematics

Computers were designed to run lots of calculations, so its easy to do maths.  You can use the following basic math functions:

- `+` add numbers together
- `-` subtract numbers from each other
- `*` multiply numbers together
- `/` divide numbers

Look at these examples of Clojure code and their equivalent math expression:

```clojure
(+ 1 1)         ;=> 1 + 1 = 2
(- 12 4 1)      ;=> 12 - 4 - 1 = 7
(* 13 2 10 2)   ;=> 13 * 2 * 10 * 2 = 520
(/ 27 9)        ;=> 27 / 9 = 3
(+ 4/3 7/8)   ;=> 53/24 Ratio value 
(- 9 4.2 1/2) ;=> 4.3   Ratio cast to Double
```

Math looks a little different in Clojure because `+`, `-`, `*`, `/` are functions, which take numbers as arguments.

!!! INFO "Number representations"
    `123` is an Integer value, `java.lang.Long`

    `3.14` is a Decimal value, `java.lang.Double`

    `22/7` is a Ratio value, `clojure.lang.Ratio` (used to preserve accuracy of data)


## Ratios

Mathematics includes the fraction, e.g. 22/7, to express what would otherwise be decimal values with potentially less accuracy.  

Clojure provides the ratio type and values can be litterally expressed as a fraction.

Computers cannot perfectly represent all floats, but ratios are always exact.

```clojure
1/2
-7/3
```

??? WARNING "Denominator must not be zero"
    The [denominator](http://en.wikipedia.org/wiki/Fraction_%28mathematics%29) of a ratio cannot be equal to `0`, as this would be equivalent to dividing by zero.


[:fontawesome-solid-book-open: Clojure Ratio type](https://practical.li/clojure/reference/clojure-syntax/ratios/){target=_blank .md-button}
[:globe_with_meridians: Fraction - Wikipedia](https://en.wikipedia.org/wiki/Fraction){target=_blank .md-button}


## Prefix notation

In Clojure, `+`, `-`, `*` and `/` appear before two numbers. This is called _prefix notation_. What you're used to seeing is called _infix notation_, as the arithmetic operator is in-between the two operands.

Languages such as **JavaScript** use **infix** notation, while **Clojure** only uses **prefix** notation.

Prefix notation is useful for many reasons. Look at this example of an infix expression and the prefix equivalent:

```clojure
Infix:  1 + 2 * 3 / 4 + 5 - 6 * 7 / 8 + 9

Prefix: (+ (- (+ (+ 1 (/ (* 2 3) 4)) 5) (/ (* 6 7) 8)) 9)
```

### Benefits of prefix notation

Imagine both the above expressions are unclear.  However in the prefix version you do not have to ever think about the [precedence of operators](https://en.wikipedia.org/wiki/Order_of_operations).

Because each expression has the operator before all the operands and the entire expression is wrapped in parentheses, all precedence is explicit.

```clojure
Infix:  1 + 2 / 3
Prefix: (+ 1 (/ 2 3))
```

### Less repetitive

Another reason prefix notation can be nice is that it can make long
expressions less repetitive.

With prefix notation, if we plan to use the same operator on many
operands, we do not have to repeat the operator between them.

```clojure
Infix:  1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9
Prefix: (+ 1 2 3 4 5 6 7 8 9)
```


## Exercises

[Start a Clojure REPL](/learn-clojure/clojure-repl/) and solve math related exercises.


### Age of Languages

Clojure was 16 years old in 2023, so its age can be represented as the number `16`

Write an expression to calculate the total number of years from all the following languages

- Clojure (16 years)
- Haskell (33 years)
- Python (32 years)
- Javascript (27 years)
- Java (28 years)
- Ruby (28 years)
- C (51 years)
- C++ (40 years)
- Lisp (65 years)
- Fortran (66 years)


!!! NOTE "Language ages as raw data"
    ```text
    16 33 32 27 28 28 51 40 65 66
    ```

??? EXAMPLE "Example solution"
    The simplest way to calculate the total is to add the age of each language together

    `clojure.core/+` adds all the ages together

    ```clojure
    (+ 16 33 32 27 28 28 51 40 65 66)
    ```
    
    As two values the same a function call to `clojure.core/*` could be used instead 

    ```clojure
    (+ 16 33 32 27 (* 28 2) 51 40 65 66)
    ```


## Average age

Calculate the average age of the 10 programming languages

Write an expression to divide the total age of all languages by 10, as there are 10 languages.

Add the ages of all the programming languages.  

Wrap that expression with another to divide that total age with the number of languages

??? EXAMPLE "Example Solution"
    ```clojure
    (/ (+ 16 33 32 27 28 28 51 40 65 66)
       10)
    ```

