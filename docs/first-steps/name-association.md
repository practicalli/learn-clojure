# Name association

`def` function associates a name (symbol) with a value, allowing the name to be used as a placeholder for the value.

!!! EXAMPLE "Define a name for a simple value"
    Define `first-name` as a name (symbol) that is associated with (bound to) the value "Jenny"

    ```clojure
    (def first-name "Jenny")
    ```
    Names (symbols) in Clojure that have multiple words use the `kebab-case` style (like a [:globe_with_meridians: shish kebab](https://en.wikipedia.org/wiki/Shish_kebab){target=_blank} where words are skewered with dashes) 

`def` has the scope of the namespace, so can with functions within that namespace or where the namespace of the def has been required.

!!! EXAMPLE "Define and use a name"
    ```clojure
    (ns practicalli.playground)

    (def language-ages [16 33 32 27 28 28 51 40 65 66])

    (/ (apply + language-ages)
       (count language-ages))
    ```
    `language-ages` is defined as a collection of number values representing the age of ten different programming languages.

    `apply` uses `+` function to add all the language ages together to get a total age

    `count` returns the number of values in the collection and that value divides the total age to get the average age.

The value in a `def` expression is evaluated, so can be a simple value, an expression or a function call.

!!! EXAMPLE "def with a calcualted value"

    ```clojure
    (def seconds-in-one-hour (partial * 60 60))
    ```

??? HINT "Values of def are cached in the REPL"
    When a `def` expression is first evaluated the value is cached in the REPL state.

    When evaluating the name created by the def, the cached value is used.  Only when the `def` expression is evaluated again is the value potentially changed.

    Where the value is an expression, the resulting value from evaluating the expresison is associated with the name.  So the name (symbol) can act as simple cache within the REPL.


!!! EXAMPLE "def with a partial function"
    ```clojure
    (def hours->seconds (partial * 60 60))
    ```
    `partial` is an example of a lazyly evaluated form, so the name `hours->seconds` will be bound to the value `(partial * 60 60)` 

     The Clojure idiom is to use an anonymous function, in this example the short form of the anonymous function.

    ```clojure
    (def hours->seconds #(* 60 60 %))
    ```

## Local names

`let` associates (binds) names to values within the scope of its own expression.  Outside the `let` expression the name is out of scope.

```clojure
(let [name value]
  name)
```

`let` is used inside a function definition to hold values, mainly to simplify the code reqiured within that function.

`let` can reduce duplication by associating the result of an expression to a name and using that name where ever that value is required.

A name can use other names associated to values in the let, as long as they preceed the name.  


!!! WARNING "avoid using def for local names"
    `let` should always be used within function definitions.

    `def` expression should not be used within a function definition as namespace scope should not be mixed in with local scope.  Use an `atom`, `println` or `tap>` if values need to be captured other than those returned by the function.

!!! HINT "use let for REPL experiments"
    `let` expression can be very useful for REPL experiments, defining a known state to evaluate expressions. 



There’s an old bit of wisdom that contends that naming things is one of the two hardest problems in programming. Difficult or not, putting names to values is something programmers do constantly. But in programs not all names are created equal. Sometimes we want to name a value that is going to stick around for a long time and we want that name to be widely visible. But sometimes we want a temporary name, a name that we can use right here, right now and then dispose of without another thought. The good news is that we have the first kind of names, the long-lasting ones, covered: that’s what def—and if you think about it, defn—are for.

So in this chapter we’ll look at let, which takes care of those local, temporary naming chores. We’ll see how, along with enabling you to mix intention-revealing names into your code, let is also surprising helpful in writing higher-order functions. We’ll also explore how if-let and when-let enable you to combine a let with an if and a when. And, as usual, we’ll look at some uses of let in real-world programs and at some of the ways you can let yourself in for trouble.


## A Local, Temporary Place for Your Stuff
We’ll begin our exploration of local naming in Clojure by imagining that our book store runs periodic specials. Every now and then we offer our customers a percentage discount on their book purchases. Unfortunately, our deal does come with some fine print: there’s a minimum charge for each order that overrides the discount.

Armed with our hard-won knowledge of functions and if, it’s not difficult to turn this discount policy into Clojure code:

let/examples.clj
​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​if​ (> (* amount (- 1.0 discount-percent)) min-charge)
​ 	    (* amount (- 1.0 discount-percent))
​ 	    min-charge))
As a bit of everyday software engineering, compute-discount-amount is a mixed bag. On the plus side, it does work. Unfortunately, compute-discount-amount is hardly a model of clarity. Come back to this code after a few months’ absence, and there’s a fair chance you’ll be muttering, Wait, what times who is greater than huh?

Clearly a bit of intention-revealing naming is called for. The code would be a lot easier to follow if we could bind a symbol—perhaps called discounted-amount—to the appropriate value and have that binding disappear once we’re done computing discounts. But how?

You might be tempted to do something like this:

​ 	​;; Don't do this!​
​ 	
​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​def​ discounted-amount (* amount (- 1.0 discount-percent)))   ​; NOOOOO!​
​ 	  (​if​ (> discounted-amount min-charge)
​ 	    discounted-amount
​ 	    min-charge))
There are two reasons that you should avoid using def like this, inside of a function. The first reason is that, as we have seen, symbols bound with def have reasonably global visibility. Calling this version of compute-discount-amount will have the ugly side effect of changing the value bound to discounted-amount, and that change will be visible outside of the function:

​ 	​;; A nasty side effect is brewing here.​
​ 	
​ 	(​def​ discounted-amount ​"Some random string."​)
​ 	
​ 	(compute-discount-amount 10.0 0.20 1.0)
​ 	
​ 	discounted-amount   ​; Is now 8.00​
The second, more philosophical reason is that using def this way is actually misusing it: def is designed to bind more or less global symbols to their more or less stable values. Instead of def you should use let for your local-naming needs:

​ 	​;; Do use let!​
​ 	
​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​let​ [discounted-amount (* amount (- 1.0 discount-percent))]
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
The mechanics of let couldn’t be simpler: you call let like a function, passing in a name and a value—wrapped in square brackets—followed by an expression. In essence you say, Execute this expression with this symbol bound to this value. In our example, the symbol was discounted-amount and the value was the order amount less the percentage discount. As you can probably figure out from the example, the value returned by the let is the value computed by the expression—the body—of the let. Critically, the bindings manufactured by let go away once the let is done, so that you can get on with the rest of the code without littering your mental landscape with stray names.

One nice feature of let is that you can bind multiple names inside a single let. We could, for example, make the code a bit clearer by doing the percentage-off calculation in two steps:

​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​let​ [discount (* amount discount-percent)
​ 	        discounted-amount (- amount discount)]
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
The rule is that let will bind each name to its corresponding value, starting with the first one. Each name becomes available immediately after it’s bound, which is why we can use discount to compute discounted-amount.

You can also have more than one expression inside the body of the let. If, for example, you wanted to print our intermediate values for debugging purposes, you could do this:

​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​let​ [discount (* amount discount-percent)
​ 	        discounted-amount (- amount discount)]
​ 	    (println ​"Discount:"​ discount)
​ 	    (println ​"Discounted amount"​ discounted-amount)
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
Keep in mind that while all the expressions in the body of a let get evaluated, only the last expression has anything to say about the value returned by the let.


## Let Over Fn
While the ideas behind let aren’t terribly challenging—it binds names to values in a purely local and temporary way—let does have some hidden superpowers, which only become visible when you combine it with fn. To get a glimpse of these hidden powers, imagine our book discounts are customer-dependent. Somewhere we have a map of the user’s name to the discount that user gets:

​ 	(​def​ user-discounts
​ 	  {​"Nicholas"​ 0.10 ​"Jonathan"​ 0.07 ​"Felicia"​ 0.05})
We could certainly add some parameters to compute-discount-amount to deal with this:

​ 	(​defn​ compute-discount-amount [amount user-name user-discounts min-charge]
​ 	  (​let​ [discount-percent (user-discounts user-name)
​ 	        discount (* amount discount-percent)
​ 	        discounted-amount (- amount discount)]
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
The trouble with this approach is that we now have to carry the “user names and discounts” table around every time we want to compute a price. If all that extra lifting turns out to be a problem, a better strategy might be to create a higher-level function, one that produces variants of the compute-discount-amount function tailored to a particular customer:

​ 	(​defn​ mk-discount-price-f [user-name user-discounts min-charge]
​ 	  (​let​ [discount-percent (user-discounts user-name)]
​ 	    (​fn​ [amount]
​ 	      (​let​ [discount (* amount discount-percent)
​ 	            discounted-amount (- amount discount)]
​ 	        (​if​ (> discounted-amount min-charge)
​ 	          discounted-amount
​ 	          min-charge)))))
​ 	
​ 	​;; Get a price function for Felicia.​
​ 	
​ 	(​def​ compute-felicia-price (mk-discount-price-f ​"Felicia"​ user-discounts 10.0))
​ 	
​ 	​;; ...and sometime later compute a price​
​ 	
​ 	(compute-felicia-price 20.0)
There are quite a few moving parts in mk-discount-price-f, but it’s just a combination of features we’ve already seen. The first thing mk-discount-price-f does, in that initial let, is look up the discount percentage for the user. With the percentage rate in hand, mk-discount-price-f then constructs an anonymous function with an fn whose body is nearly identical to compute-discount-amount.

The interesting thing about mk-discount-price-f is how discount-percent gets bound in the initial let, outside of the fn and then used inside the fn. That means that while discount-percent is only visible inside the body of the let, it can live on long after the call to mk-discount-price-f has completed, buried inside of the anonymous function.

This compute it in a let, use it in an fn is such a great way to build anonymous functions that are both efficient and clear. It’s efficient because you can use the outside let to compute everything you need to construct the anonymous function. And it’s clear because inside of the anonymous function you can use descriptive names for those precomputed values.


## Variations on the Theme
In addition to the plain vanilla version of let that we’ve looked at so far, Clojure comes packaged with a couple of handy variations. The most commonly used of these is probably if-let. As you might guess from the name, if-let is an if and a let rolled into one. To see if-let in action, imagine that we decide to represent anonymous books with our now-familiar book map, sans the :author key:

​ 	(​def​ anonymous-book
​ 	  {:title ​"Sir Gawain and the Green Knight"​})
​ 	
​ 	(​def​ with-author
​ 	  {:title ​"Once and Future King"​ :author ​"White"​})
Now imagine we needed to write a function that will return the uppercase version of the author’s name, or nil if there is no author. The twist is that we need to avoid computing the uppercase version of nil, which will blow up with an exception. Given that, we might do something like this:

​ 	(​defn​ uppercase-author [book]
​ 	  (​let​ [author (:author book)]
​ 	    (​if​ author
​ 	      (.toUpperCase author))))
That will work, but we can say it a bit more succinctly with if-let:

​ 	(​defn​ uppercase-author [book]
​ 	  (if-let [author (:author book)]
​ 	    (.toUpperCase author)))
In essence, if-let takes a single binding and uses the value bound—in the example, the author’s name—as the condition of an if. Like a plain if, if-let will take a second expression, for the else case:

​ 	(​defn​ uppercase-author [book]
​ 	  (if-let [author (:author book)]
​ 	    (.toUpperCase author)
​ 	    ​"ANONYMOUS"​))
And if you think it would make more sense to call it let-if, well, me too.

Unsurprisingly, there is also a when-let which does about what you would expect:

​ 	(​defn​ uppercase-author [book]
​ 	  (when-let [author (:author book)]
​ 	    (.toUpperCase author)))
There really is nothing terribly deep about if-let and when-let: They are just the kinds of things that grow out of the observation that people frequently combine let with if and when.



## In the Wild
Real-life Clojure functions are full of let expressions. In fact, let is one of the most commonly used Clojure features, up there with defn and def. If, for example, you look at the Ring source code you will find the parse-params function. Here’s a slightly simplified version of it:

​ 	(​defn​ parse-params [params encoding]
​ 	  (​let​ [params (codec/form-decode params encoding)]
​ 	    (​if​ (map? params) params {})))
Without diving into the belts and pulleys of Ring, we can deduce that parse-params decodes some raw parameter data into a value that is either a map or something else, presumably nil. Once it has the result of that decoding bound to params—courtesy of let—it proceeds to return the decoded value if it is indeed a map, or an empty map if it’s not.

If you dig around in Ring some more you’ll discover the assoc-query-params function, which uses parse-params. Here is a slightly simplified version of that function, which has a vanilla let embedded in an if-let:

​ 	(​defn​ assoc-query-params
​ 	  ​"Parse and assoc parameters from the query string​
​ 	​  with the request."​
​ 	  [request encoding]
​ 	  (merge-with merge request
​ 	    (if-let [query-string (:query-string request)]
​ 	      (​let​ [params (parse-params query-string encoding)]
​ 	        {:query-params params, :params params})
​ 	      {:query-params {}, :params {}})))
Pull the query string out of the request and call it query-string and, if you actually got something, proceed to parse the parameters and call the result in params and then … well, you get the picture.

For a truly imposing example of a let, we need to look no further than this, from Incanter[14]: ]

​ 	(​let​ [opts (​if​ options (apply assoc {} options) {})
​ 	      data (or (:data opts) $data)
​ 	      _x (data-as-list x data)
​ 	      nbins (or (:nbins opts) 10)
​ 	      theme (or (:theme opts) :default)
​ 	      density? (true? (:density opts))
​ 	      title (or (:title opts) ​""​)
​ 	      x-lab (or (:x-label opts) (str ​'x​))
​ 	      y-lab (or (:y-label opts)
​ 	                 (​if​ density? ​"Density"​ ​"Frequency"​))
​ 	      series-lab (or (:series-label opts) (str ​'x​))
​ 	      legend? (true? (:legend opts))
​ 	      dataset (HistogramDataset.)]
​ 	
​ 	  ​;; Do something heroic with x-lab and density?​
​ 	  ​;; and title and...​
​ 	  )
The preceding code, which is used in drawing histograms, binds no less then a dozen names. Glossing over the details—which thankfully need not concern us here—we can see that opts, which is defined right out of the gate, is used nine times later on in the let. Look a little more closely, and you can see that the value of opts comes out of an if expression, while the value of y-lab is computed with an if embedded in an or. Behold the power of an expression-based language.

The other thing to behold is how much computing gets done inside the square brackets of this let. There are ifs and ors and a number other function calls going off in there. The lesson here is that if you have an intricate set of step-by-step values to compute, consider doing it inside the square brackets of let, giving each intermediate result an informative name. Your future self will thank you.



## A Local, Temporary Place for Your Stuff

We’ll begin our exploration of local naming in Clojure by imagining that our book store runs periodic specials. Every now and then we offer our customers a percentage discount on their book purchases. Unfortunately, our deal does come with some fine print: there’s a minimum charge for each order that overrides the discount.

Armed with our hard-won knowledge of functions and if, it’s not difficult to turn this discount policy into Clojure code:

let/examples.clj
​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​if​ (> (* amount (- 1.0 discount-percent)) min-charge)
​ 	    (* amount (- 1.0 discount-percent))
​ 	    min-charge))
As a bit of everyday software engineering, compute-discount-amount is a mixed bag. On the plus side, it does work. Unfortunately, compute-discount-amount is hardly a model of clarity. Come back to this code after a few months’ absence, and there’s a fair chance you’ll be muttering, Wait, what times who is greater than huh?

Clearly a bit of intention-revealing naming is called for. The code would be a lot easier to follow if we could bind a symbol—perhaps called discounted-amount—to the appropriate value and have that binding disappear once we’re done computing discounts. But how?

You might be tempted to do something like this:

​ 	​;; Don't do this!​
​ 	
​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​def​ discounted-amount (* amount (- 1.0 discount-percent)))   ​; NOOOOO!​
​ 	  (​if​ (> discounted-amount min-charge)
​ 	    discounted-amount
​ 	    min-charge))
There are two reasons that you should avoid using def like this, inside of a function. The first reason is that, as we have seen, symbols bound with def have reasonably global visibility. Calling this version of compute-discount-amount will have the ugly side effect of changing the value bound to discounted-amount, and that change will be visible outside of the function:

​ 	​;; A nasty side effect is brewing here.​
​ 	
​ 	(​def​ discounted-amount ​"Some random string."​)
​ 	
​ 	(compute-discount-amount 10.0 0.20 1.0)
​ 	
​ 	discounted-amount   ​; Is now 8.00​
The second, more philosophical reason is that using def this way is actually misusing it: def is designed to bind more or less global symbols to their more or less stable values. Instead of def you should use let for your local-naming needs:

​ 	​;; Do use let!​
​ 	
​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​let​ [discounted-amount (* amount (- 1.0 discount-percent))]
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
The mechanics of let couldn’t be simpler: you call let like a function, passing in a name and a value—wrapped in square brackets—followed by an expression. In essence you say, Execute this expression with this symbol bound to this value. In our example, the symbol was discounted-amount and the value was the order amount less the percentage discount. As you can probably figure out from the example, the value returned by the let is the value computed by the expression—the body—of the let. Critically, the bindings manufactured by let go away once the let is done, so that you can get on with the rest of the code without littering your mental landscape with stray names.

One nice feature of let is that you can bind multiple names inside a single let. We could, for example, make the code a bit clearer by doing the percentage-off calculation in two steps:

​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​let​ [discount (* amount discount-percent)
​ 	        discounted-amount (- amount discount)]
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
The rule is that let will bind each name to its corresponding value, starting with the first one. Each name becomes available immediately after it’s bound, which is why we can use discount to compute discounted-amount.

You can also have more than one expression inside the body of the let. If, for example, you wanted to print our intermediate values for debugging purposes, you could do this:

​ 	(​defn​ compute-discount-amount [amount discount-percent min-charge]
​ 	  (​let​ [discount (* amount discount-percent)
​ 	        discounted-amount (- amount discount)]
​ 	    (println ​"Discount:"​ discount)
​ 	    (println ​"Discounted amount"​ discounted-amount)
​ 	    (​if​ (> discounted-amount min-charge)
​ 	      discounted-amount
​ 	      min-charge)))
Keep in mind that while all the expressions in the body of a let get evaluated, only the last expression has anything to say about the value returned by the let.


## Wrapping Up

In this chapter we had a look at the let expression and its friends if-let and when-let. We saw how let and company enable you to bind a name to a value as you compute it, a binding that lasts only as long as you need it. We’ve also seen how you can use let to organize and illuminate long sequences of computing by giving a name to each intermediate value. As usual, we also looked at some of the programming pits that you can fall into when using let—and happily discovered that those pits were both few in number and fairly easy to avoid.

Now that you know all about let, it’s time to dig deeper into Clojure’s other naming mechanism, def.

FOOTNOTES
[14]
http://incanter.org


