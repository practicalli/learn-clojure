# celebrity-name-smash

Take celebrity names and smash them together to create weird and wonderful gestalt version of them.

To understand how this code was created, read the article [Clojure Dojo: Celebrity Name Smash - June 2016](http://jr0cket.co.uk/2016/06/clojure-dojo-celebrity-name-smash-June-2016.html)




> ####TODO::Break the celebrity name smash challenge into several sections


The June 2016 edition of the London Clojurians coding dojo set the challenge of building a celebrity name smash, taking two "celebrities" and smashing their names together to make a weird or amusing gestalt name.

For bonus points the challenge would include this celebrity name smash as a service and even more bonus points if using the new `clojure.spec` library to put specifications around data structures and functions.

> Bonus points are non-redeemable, sorry!

Although our group didn't get get any of the bonus levels, here is the blow by blow development of our code for the Celebrity Name Smash.

<!-- more -->

[![Join in the fun of functional programming with the London Clojurians](/images/london-clojurians-banner.png)](http://www.meetup.com/London-Clojurians/)

# A simple leiningen project to start

We created a default Clojure project to start using the following leiningen command

```
lein new celebrity-name-smash
```

This created a simple project using Clojure 1.8.0.  If we had chosen to use `clojure.spec` as well then we would have updated the `project.clj` file to use Clojure 1.9.x as a dependency instead.

# Modeling the Celebrities

The simplest way to represent a celebrity name is in a string.  So we bound a name called `celebrities` to a string containing the first celebrity we could think of

```clojure
(def celebrities "Brad Pitt")
```

As we want to have two celebrities then we changed the data structure into a Clojure vector.  A vector is the most flexible data structure in Clojure.  So we redefined the name `celebrities` to be bound to a vector of strings containing the first celebrity couple we could think of.

```clj
(def celebrities ["Brad Pitt" "Angelina Jolie"])
```

# Splitting names

Each celebrity has a first and last name, so we need to split them into individual strings first.

> We decided to exclude celebrities with just a single name.

From a quick Google we found the [clojure.string/split](https://clojuredocs.org/clojure.string/split) function that will split a string on a given pattern, that pattern being a regular expression (regex).

```clojure
(clojure.string/split "Clojure is awesome!" #" ")

;; => ["Clojure" "is" "awesome!"]
```

The regular expression pattern `" "` matches the space characters.  We could have also used `#"+s"` for the same results in this example, although it was felt that the space was clearer in intent.

So we wrote a function called `name-split` to take a first and last name as a string and return two separate strings, one for the first name and one for the last name.


```clojure
(defn name-split
  "Splits a celebrity name into their first & last names"
  [name]
  (str/split name #" "))
```

We tested the `name-split` function in the repl

```clojure
(name-split "Brad Pitt")

;; =>["Brad" "Pitt"]
```

We could now successfully split the full name of a celebrity into their first and last names.

> A more advanced example of splitting up words would be to use re-seq with a regex patter, as in the [HHGTTG book processing example in clojure-through-code](https://github.com/practicalli/clojure-through-code/blob/master/src/clojure_through_code/xx-hhgttg-book.clj).


# Creating sub-strings of names the long way

As the aim of our code is to create silly and weird names from celebrity names, we wont get the desired results with just the first and last names.  So we take those and split them.

At first we decided to split them in half, rounding down for odd length names.

As a Clojure String can be used like a collection of characters, we could simply `take` the first x number of characters.

```clojure
(take 2 "Brad")
;; => (\B \r)
```

The value returned is a list of characters, so we would have to combine them back into a string.  Just using the `str` function on the result of the `take` function returned a lazy sequence.  To get a string we needed to `apply` or `reduce` with the `str` function

```clojure
;; (str (take 2 "Brad"))
;; => "clojure.lang.LazySeq@c31"

(apply str (take 2 "Brad"))   ;; => "Br"
(reduce str (take 2 "Brad"))  ;; => "Br"
```

To do this for a name of any length we would need to `count` the string characters and divide by 2.

```clojure
(take (/ (count "Brad") 2) "Brad")
;; => (\B \r)

(apply str (take (/ (count "Brad") 2) "Brad"))
;; => "Br"
```

This code also works for names that have an odd number of characters.  When the odd number of characters is divided by two, a Clojure ratio type is used to hold the result rather than return a decimal value.  The `take` function calculated the value of the ratio type and rounds it to the nearest whole number.

Here is a breakdown of how this code works with a name containing an odd number of characters.

```clojure
(count "Bradley")
;; => 7

(/ (count "Bradley") 2)
;; => 7/2

(take (/ (count "Bradley") 2) "Bradley")
;; => (\B \r \a \d)

(apply str (take (/ (count "Bradley") 2) "Bradley"))
;; => "Brad"
```

# Creating sub-strings the short way

After reviewing this code it seemed a little complex for what we wanted, so a quick Google gave us the `subs` function.  The `subs` function takes a string and a starting point for the split, with an optional end point

```clojure
(subs "Brad" 0 2)
;; => "Br"

(subs "Brad" 2)
;; => "ad"

(subs "Bradley" 0 (/ (count "Bradley") 2))
;; => "Bra"
```

So when we want the first part of the name we give the `subs` function a start point and an end point for the sub-division.  For the last part of a name we simply give the start point for the sub-division.


> **Hint** If the `take` or `subs` function did not deal with odd numbers of characters, then instead of dividing by 2 we could have used the `quot` function.  The `quote` function divides the first argument by the second argument, returning the result as a whole number.


# Creating a subname function for the first name

We created a function that takes the name as a argument and returns the substring for the first half of the name

```clojure
(defn first-celeb-subname [name]
  (let [end (/ (count name) 2)]
    (subs name 0 end)))

;; calling the function with an odd length name
(first-celeb-subname "Bradley")
;; => "Bra"
```

We used the `let` function to create a local name (symbol) called `end` that points to the end position in the string, based on dividing the name by 2.  Then we call the `subs` function to get the substring from 0 to the value of `end`.


# Creating a more random sub-name function

Just talking the half way point for our substring only gives one result.  If we add a random element to creating our substring then we should get many more variations in results.

```clojure
(defn first-celeb-subname [name]
  (let [end (+ 1 (rand-int (count name)))]
    (subs name 0 end)))
```

A slight refinement can be made by replacing `+ 1` with the `inc` function

```clojure
(defn first-celeb-subname [name]
  (let [end (inc (rand-int (count name)))]
    (subs name 0 end)))
```

# A subname function for the second celebrity

We wanted to combine two first names and two last names to make a new first & last name.  So we need a similar function to create the lastname subname

```clojure
(defn last-celeb-subname [name]
  (let [start (inc (rand-int (count name)))]
    (subs name start)))
```

This function is almost identical to the first function, however only a start position is provided to `subs` function, creating a substring from the `start` position to the end of the name.


# Putting it all together

Finally we call these functions from a main function named `celeb-name-smash`, which takes two celebrity names as string arguments and returns a string containing the smashed name.

```clojure
(defn celeb-name-smash
  "Smashes to celebrity names together"
  [celeb-one celeb-two]
  (str (first-celeb-subname (first (name-split celeb-one)))
       (second-celeb-subname (first (name-split celeb-two)))
       " "
       (first-celeb-subname (last (name-split celeb-one)))
       (second-celeb-subname (last (name-split celeb-two)))))

;; call the celeb-name-smash function with example data
(name-smash-strings "Brad Pitt" "Angelina Jolie")
;; => "Brana Pitie"
;; => "Brelina Pie"
;; => "Brelina Piolie"
;; => "Brlina Pi"
```

The `celeb-name-smash` function has a lot of duplication, so should probably be refactored to make it more elegant.  However, we ran out of time at the dojo, so I will have a look at refactoring this function as homework.

T
