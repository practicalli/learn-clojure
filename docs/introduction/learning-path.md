# Clojure Learning Path

Practicalli recommended approach for learning Clojure, which can be done in parallel.

- Become comfortable evaluating code in the [Clojure REPL](#clojure-repl)
- Review the fundamental [syntax of Clojure](#experience-the-clojure-syntax) (its small and consistent)
- Configure a [REPL connected Editor](#repl-connected-editor) and create projects to save experiments
- Start discovering [clojure concepts](#clojure-concepts) (give context to practical experience)
- [Practice Clojure](#practice-practice-practice) by solving challenges and discover more about the Clojure Standard Library (hundreds of functions available)
- Connect to the [Clojure community](#community-help) for advice and shared experiences


## Clojure REPL 

Practice writing and evaluating Clojure code using [:fontawesome-solid-book-open: a simple REPL prompt](/learn-clojure/clojure-repl/) that also provides help and documentation for standard Clojure functions.


## Experience the Clojure syntax

Take a quick look at the Syntax of Clojure.  The syntax is very small, so this will take about 15 minutes to 1 hour (dependent on your own experiences with coding).  Don't try to remember all the syntax and functions, they will come through practise.

- eg. [:fontawesome-solid-book-open: Clojure in 15 minutes](clojure-in-15-minutes.md)


## REPL Connected Editor

Build on the REPL experience by connecting a Clojure aware editor to the Clojure REPL process. 

A REPL connected Editor provides an effective set of Clojure development tools to evaluate Clojure expressions.   

Practicalli provides editor install and usage guides for 

- [:fontawesome-solid-book-open: Emacs using Spacemacs community configuration](https://practical.li/spacemacs/){target=_blank} 
- [:fontawesome-solid-book-open: Neovim using AstroNvim community configuration](https://practical.li/neovim/){target=_blank} 

[:fontawesome-solid-book-open: Practicalli overview of Clojure Aware Editors](https://practical.li/clojure/clojure-editors/){target=_blank .md-button} 

!!! HINT "Web based Clojure environments"
    [Exercism](/learn-clojure/code-challenges/exercism/) includes a web-based editor for solving its challenges (and challenges can be downloaded locally).

    [:globe_with_meridians: repl.it](https://repl.it) provides web based repl you can share / fork via a GitHub account.


## Clojure Concepts

Gain an appreciation that a software system should strive for a simple design is a crucial step to truly understanding Clojure.  

Spend an hour watching the author of the Clojure Language, [:globe_with_meridians: Rich Hickey, talk about Simple made Easy](https://www.infoq.com/presentations/Simple-Made-Easy) or read the ([:globe_with_meridians: transcript of talk](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/SimpleMadeEasy.md)) to emerse in the foundational concepts of Clojure.

Review the [:fontawesome-solid-book-open: Clojure Big Ideas](concepts/) presented by Stuart Halloway and further [:fontawesome-solid-book-open: video presentations by Rich Hickey](concepts/clojure-from-the-author.md){target=_blank .mkdocs-button}.

[:fontawesome-solid-book-open: Rich Hickey video lecture series](concepts/clojure-from-the-author.md){target=_blank .mkdocs-button} 


## Practice Practice Practice

Practice Clojure.  Write lots of small and relatively simple examples in Clojure and experiment with the code in the REPL.  

Regular practice helps learn and retain the many of the 700+ functions within the [Clojure Standard API](https://clojure.github.io/clojure/)

Aim to become comfortable in the understanding of:

- basic values (strings, numbers, etc) and persistent collections (list, vector, map, set)
- binding names to values and their scope  (def, defn, let)
- calling functions, defining functions, arity options for functions
- Higher order functions and basics of functional composition (map, reduce, filter, etc)
- Designing with data, Extensible Data Notation (EDN), data manipulation

Activities to help practice Clojure include:

- [4ever Clojure](/learn-clojure/code-challenges/4ever-clojure/) - aim to complete the first 50 exercises and experiment with various functions from the Clojure standard library (`clojure.core`).
- [Exercism Clojure Track](/learn-clojure/code-challenges/exercism)
- [Code Kata](/learn-clojure/code-challenges/code-kata) repeat exercises taking different design decisions 
- [Small Projects](/learn-clojure/small-projects)


## Community Help

There are many ways to [:simple-livejournal: get help from the Clojure community](https://practical.li/blog/posts/cloure-community-getting-help/)

Often starting to ask questions of the community is an effective way of solving the problem yourself.  Asking specific questions helps the community help you.  Posting the specifics of the solution helps the community grow.

Clojurians Slack community is very active and the Clojurians Zulip community is specifically active around data science.

??? HINT "In-person Code Dojo events"
    A local Clojure community may run [Code Dojo events](https://londonclojurians.org/code-dojo/) which are an excellent way to learn and practice with others. e.g. [London Clojurians](https://meetup.com/london-clojurians)

    The Clojure dojo is a collaborative way to learn Clojure/ClojureScript through practice. The aim is to learn a little more than before you started.

    Collectively decide on a challenge to complete and split into small groups (2-4 people).  Discuss and start to solve the challenge by coding the next simplest thing possible.  Spend around 90 minutes in groups and come back together to show what was learned.  The dojo is about sharing lessons learned rather than completing a challenge.


## Building a frame of reference

Find an introductory book that you like which provides lots of example code to help you feel more comfortable with the syntax and more importantly the major concepts of functional programming with Clojure.  Type in the exercises as you read and don't be afraid to play around with code examples

[Clojure.org Book page](https://clojure.org/community/books){target=_blank .md-button} has a comprehensive list of commercially available books 

Freely available books Practicalli recommends:

[:fontawesome-solid-book-open: Practicalli Learn Clojure](/learn-clojure/){target=_blank .md-button}

[:globe_with_meridians: Clojure Cookbook](https://github.com/clojure-cookbook/clojure-cookbook){target=_blank .md-button}

[:globe_with_meridians: Clojure for the Brave and the True](https://www.braveclojure.com/clojure-for-the-brave-and-true/){target=_blank .md-button}

Commercial books Practicalli recommends:

[:globe_with_meridians: Getting Clojure - Russ Olsen](https://pragprog.com/titles/roclojure/getting-clojure/){target=_blank .md-button}

[:globe_with_meridians: Clojure Essential Reference - Renzo Borgatti](https://www.manning.com/books/clojure-the-essential-reference){target=_blank .md-button}


## Starter Projects

Work on a relatively small project that is care about enough to invest time on regularly, either several hours over the weekend or a couple of hours over several days in the week.

- eg. a tool to help you at work

[:fontawesome-solid-book-open: Practicalli Small Projects](/learn-clojure/small-projects/){target=_blank .md-button} 

