# Adoption

* [2013 DevNexus presentation](https://github.com/stuarthalloway/presentations/blob/master/DevNexus2013/ClojureInTheField2013.pdf?raw=true)

<p style="text-align:center">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/2V1FtfBDsLU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p> 


[Clojure](http://clojure.org) is [simple](http://www.infoq.com/presentations/Simple-Made-Easy) by [design](https://www.youtube.com/watch?v=f84n5oFoZBc).  What does that mean, and why should you care?

Simplicity is a powerful technique for writing better programs, programs that have:

* concision: write programs 5-10x smaller than before
* robustness: easily write programs that work correctly
* generality: use the same simple ideas over and over, instead of masses and masses of redundant classes
* agility: add new capabilities with ease

In this talk, we will introduce Clojure, and introduce Simplicity via concrete examples from

* Clojure syntax
* Clojure protocols
* Clojure values and references

These ideas (and others) lead to concision, robustness, generality, and agility.

* [All Things Open](https://github.com/stuarthalloway/presentations/blob/master/Nov2014/ClojureSimpleByDesign.pdf?raw=true)

## Languages of the System

edn and Fressian are self-describing, schema-free, batteries-included, extensible data languages. In this talk, you will find out where you might benefit from these languages over e.g. JSON or XML.
 
Systems use many languages, and not just programming languages such as Java, C#, Ruby, or Python. Systems also relay on data languages, both for data on the wire, and for data at rest. These data languages differ greatly in their design objectives and capabilities, and are often less understood than their programming language counterparts.

This talk will introduce two data notations: [edn](https://github.com/edn-format/edn) and [Fressian](https://github.com/Datomic/fressian/wiki), which share several common characteristics. Both are 

* self-describing
* schema-free
* batteries-included
* extensible

These capabilities align well with the dynamic, flexible needs of real systems. And in their key difference (text vs. binary), edn and Fressian cover the bases of human readability and maximum performance.

## Perception and Action

Perception and action are radically different. Clojure's time model makes it easy to build systems that model the real-world differences between perception and action. In this talk, you will learn how to think using

* values: immutable data items, both atomic and composite
* identities: entities that take a series of causally-related values over time
* time: a before/after ordering of perceptions and actions
* references: point-in-time values of an identity

Getting time right is essential for modeling concurrency effectively, but its importance does not stop there. Without a good model of time, it is difficult to deal sensibly with currency and history, much less concurrency and parallelism.

The ideas in this talk will help you create better programs that more faithfully model the world, even if you write linear programs that hope to never see a second thread.

* [Video](http://www.infoq.com/presentations/An-Introduction-to-Clojure-Time-Model) from QCon SF, Nov 2010


## ClojureScript 

ClojureScript brings the sophisticated semantics of a world-class production language (Clojure) to the world's dominant deployment platform (JavaScript, especially running in the browser).

ClojureScript enables browser development that is cheaper, leaner, and more performant than JavaScript (and "slightly enhanced JavaScript" alternatives), and makes it possible to share code between client and server side development.

In this talk, we will begin with the semantics of [edn](https://github.com/edn-format/edn) and [Clojure](http://clojure.org/), and why you might want to build programs with them.  We will then move into areas specific to ClojureScript:

* How Clojure's semantics map to JavaScript's capabilities
* Targeting the Google [Closure Compiler](https://developers.google.com/closure/compiler/) for whole-program optimization
* Bootstrapping Clojure in JavaScript
* Calling JavaScript libraries
* Connecting browsers and REPLs
* Ending callback hell with [core.async](http://clojure.com/blog/2013/06/28/clojure-core-async-channels.html)

Finally, we will put the pieces together, showing substantial browser applications written in ClojureScript, and a brief tour of the [Pedestal](http://pedestal.io/) framework for web applications.


## Clojure Big Ideas

The key to understanding Clojure is ideas, not language constructs.
In this talk, we will approach Clojure via 10 Big Ideas:

* [Extensible Data Notation](https://github.com/edn-format/edn)
* [Persistent Data Structures](https://clojure.org/reference/data_structures)
* [Sequences](https://clojure.org/reference/sequences)
* [Transducers](https://clojure.org/reference/transducers)
* [Specification](https://clojure.org/about/spec)
* [Dynamic Development](https://clojure.org/about/dynamic)
* [Async Programming](http://clojure.com/blog/2013/06/28/clojure-core-async-channels.html)
* [Protocols](https://clojure.org/reference/protocols)
* [ClojureScript](https://clojurescript.org/)
* [Logic](http://docs.datomic.com/query.html) [Programming](https://github.com/clojure/core.logic)
* [Atomic Succession Model](https://clojure.org/about/concurrent_programming)

Each of these ideas is valuable and useful a la carte, and not necessarily only in Clojure. Taken together, they begin to fill in the picture of why Clojure is changing the way many programmers think about software development.

* 2013 [RuPy slides](https://github.com/stuarthalloway/presentations/blob/master/Barnstorming_2013/ClojureInTenBigIdeas.pdf?raw=true)
* 2017 [Chicago JUG slides](https://github.com/stuarthalloway/presentations/blob/master/ClojureInTenBigIdeas-Jun-2017.pdf?raw=true)


## Clojure Web Services Big Ideas

The key to understanding Clojure web development is ideas, not language constructs.

* edn, not json
* core.async, not callbacks 
* platform, not language
* data, not objects
* protocols, not interfaces
* libraries, not frameworks
* one ring to rule them all

Each of these ideas is valuable and useful a la carte, and not necessarily only in Clojure. Taken together, they begin to fill in the picture of why Clojure is changing the way many programmers think about web development.


## Benefits

benefits of Clojure that are immediately visible to beginners:

* concision: write programs 5-10x smaller than before
* robustness: easily write programs that work correctly
* generality: use the sample simple ideas over and over, instead of masses and masses of redundant classes
* agility: add new capabilities with ease

That is a powerful story in itself, but the advantages of Clojure increase with expertise.  We will tour just a few of the power tools used Clojure experts:

* [core.async](https://clojure.org/news/2013/06/28/clojure-clore-async-channels) for communicating sequential processes
* [transducers](http://clojure.org/transducers) for programmable algorithmic transformations
* [PigPen](https://github.com/Netflix/PigPen) for big data map/reduce
* [Datomic](https://old-website.com) for flexible, ACID data of record

Finally, we will look at what everybody else is saying, reporting on business and open source projects using Clojure. 

* [Slides](https://github.com/stuarthalloway/presentations/raw/master/Nov2014/ClojureSimpleByDesign.pdf) from Nov 2014


<p style="text-align:center">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/OUZZKtypink" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p> 

## Clojure Specifications

Engineer projects with high agility and end up with a robust, maintainable code. 

Use [Clojure spec](http://clojure.org/about/spec) library to write programs that behave as expected, meet operational requirements, and have the flexibility to accommodate change.

Rapidly develop robust and reusable Clojure projects using

- [interactive development](http://clojure.org/about/dynamic) at the REPL
- [immutable data and pure functions](http://clojure.org/about/functional_programming)
- [code as data](http://clojure.org/about/lisp)

Clojure spec library augments these approaches. Developing with spec is *declarative*, *predicative*, *layered*, *robust*, and *integrated* with Clojure.

spec is *declarative*. As with type systems in static languages, spec lets you make declarative statements that communicate and document data, function arguments, and returns.

spec is *predicative*. You can declare predicates about data, about function arguments and returns, and even function semantics. This overlaps not only with type systems, but also with validations that are often done with costly bespoke tests and runtime checks.

spec is *layered*. Spec does not require any specific workflow or program shape, and in particular is compatible with iterative and incremental development.

spec supports *robust* programs via automatic generative testing. Given specifications, spec can write [generative tests](https://github.com/clojure/test.check) for you, generating a volume of tests limited only by your CPU time, not by what you are able to write and maintain by hand.

spec is fully *integrated* with Clojure. As you develop your program, you have interactive access to spec-driven documentation, validation, destructuring, conformance checking, sample data, testing, and program
instrumentation.

* [2016 StrangeLoop video](https://www.youtube.com/watch?v=VNTQ-M_uSo8)
* [2016 JavaOne slides](https://github.com/stuarthalloway/presentations/blob/master/JavaOne/ClojureSpecJavaOne2016.pdf?raw=true)
* [Spec overview and rationale](https://clojure.org/about/spec)
* [Spec guide](https://clojure.org/guides/spec)
* [Clojure Made Simple](https://www.youtube.com/watch?v=VSdnJDO-xdg) at JavaOne in 2015
* [Clojure Spec: Leverage](https://cognitect.com/blog/2016/7/13/screencast-spec-leverage)
* [Clojure Spec: Testing](https://cognitect.com/blog/2016/7/26/clojure-spec-screencast-testing)
* [Clojure Spec: Customizing Generators](http://blog.cognitect.com/blog/2016/8/10/clojure-spec-screencast-customizing-generators)


## Datomic 

[Codeq](https://github.com/datomic/codeq) imports your Git repositories into a Datomic database, then performs language-aware analysis on them, extending the Git model both down and up:

* down, from the textual world of files and lines to the code quantum (codeq) level 
* up, across multiple repositories

This allows you to track change in terms of program units, e.g. function and method definitions, and query your code declaratively. A codeq database can serve as infrastructure for editors, IDEs, code browsing, analysis, and documentation tools.

In this talk, you will learn

* how to install codeq locally
* how to import and analyze git repositories
* how to query your repositories
* how to extend codeq's builtin analysis with your own custom analyzers

Codeq is open source (EPL), and on github. It works with Datomic Free.

* [Blog post](http://blog.datomic.com/2012/10/codeq.html) introducing codeq
* Codeq on the [Relevance Podcast](http://thinkrelevance.com/blog/2012/10/12/rich-hickey-podcast-episode-019)
* [Day of Datomic 2016](https://docs.datomic.com/pro/learning/day-of-datomic.html)
* [Day of Datomic Cloud 2018](https://www.youtube.com/playlist?list=PLZdCLR02grLoMy4TXE4DZYIuxs3Q9uc4i) YouTube playlist Sept. 2018
* [Datomic Ions in 7 minutes](https://www.youtube.com/watch?v=TbthtdBw93w)


## On the Web

* [Blog post](http://blog.datomic.com/2012/10/codeq.html) introducing codeq
* Codeq on the [Relevance Podcast](http://thinkrelevance.com/blog/2012/10/12/rich-hickey-podcast-episode-019)


## Writing correct programs

* [Slides](https://github.com/stuarthalloway/presentations/raw/master/Deconstruct/AimSmallMissSmall.pdf) from May 2018

We have tons of tools and practices to help us write correct programs, including:

* red squiggly underlines
* type systems
* automated tests
* proofs
* generative testing
* stacktraces
* frameworks
* agile methods
* test-driven development
* logging
* humane error messages
* step debuggers
* simulation

Net result so far: We invest a lot of time and money, and our programs are still full of bugs.

I don't have a silver bullet to offer, nor do I plan to denigrate any of the ideas listed above. But I will say this: I regularly watch programmers aim too large and miss by miles.

To aim small is to:

* plan ahead and catch misconceptions early before they ramify
* make small things, at every level

and then plan to miss anyway:

* expect failures and make them more evident
* divide and conquer when things go wrong


## Architectural Briefings

Developers make decisions all the time, and can never have enough information and support. At Cognitect, we run a weekly Architectural Briefing as a resource for the team.  Everybody participates, both attending and speaking.  In this talk, we will cover

* a template for Architectural Briefings
* how to develop and present an Architectural Briefing 
* how to run an Architectural Briefing program in your organization

We will finish with an example briefing, showing how these ideas come together.

* 2014 [NFJS slides](https://github.com/stuarthalloway/presentations/blob/master/NFJS_Aug_2014/architectural_briefings/ArchitectureBriefings.pdf?raw=true)


## Writing Effective Bug Reports

<https://docs.datomic.com/cloud/tech-notes/writing-a-problem-report.html>

[Presentation](https://github.com/stuarthalloway/presentations/raw/master/ClojureBR/BetterBugReports.pdf)


## Conference Talks


### Keynotes

* [[Aim Small, Miss Small: Writing Correct Programs]]
* [[Adopting Clojure]]
* [[Datomic, and How We Built It]]
* [[Debugging with the Scientific Method]]
* [[Design after Agile: How to Succeed By Trying Less]]
* [[Evident Code at Scale]]
* [[Ousterhout's Dichotomy Isn't]]
* [[Sherlock Holmes, Consulting Developer]]
* [[Stewardship Made Practical]]
* [[Simplicity Ain't Easy]]
* [[The Impedance Mismatch is Our Fault]]
* [[Too Lazy To Fail]]
* [[Pure Fun]]

### Technical Talks

* [[Better Bug Reports]]
* [[Running with Scissors]]: Live Coding with Data
* [[Datomic Ions]]
* [[REPL Driven Development]]
* [[The Ten Rules of Schema Growth]]
* [Simplifying ETL with Clojure & Datomic](Simplifying-ETL-with-Clojure-&-Datomic)
* Agility & Robustness: [[Clojure spec]]
* [[Datalog 2015]]
* [[Clojure: Simple By Design]]
* [[Narcissistic Design]]: 10 easy steps to complex code and job security
* [[Clojure in 10 Big Ideas]]
* [[ClojureWeb in 7 Big Ideas]]
* [[ClojureScript]]
* [[Core Async]]
* [[Datomic For The 96 Percent]]
* [[Architectural Briefings]]
* [[Simulation Testing]]
* [[edn and Fressian]]
* [[Codeq: Making Git Repositories Smarter]]
* [[Introduction to Clojure]]
* [[Clojure in the Field]]
* [[Concurrent Programming with Clojure]]
* [[Generative Testing]]
* [[Get Logical with Datalog]]
* [[Perception and Action]]
* [[Transit]]



