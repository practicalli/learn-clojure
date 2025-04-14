# Namespace Design

In a small project with only a few developers, things like naming and style conventions don’t matter all that much, because almost everyone has worked with almost all of the code.

With larger teams and sizable code bases — think tens of developers, tens of thousands of lines of Clojure — there’s a good chance that anyone reading the code has never seen it before. For that reader, a few conventions can be a big help.

Optimizing for readability usually means being more verbose. Don’t abbreviate unless you have to.

Optimizing for a reader who is not necessarily familiar with the entire code base, or even an entire file. They’ve just jumped to a function definition in their editor, or maybe pulled a line number from a stack trace. They don’t want to take the time to understand how all the different namespaces relate. They especially don’t want to have to scroll to the top of the file just to see where a symbol comes from.

So these conventions are about maximizing readability at the level of single function definitions. Yes, it means more typing. But it makes it much easier to navigate a large codebase maintained by multiple people.

As a general first rule, make the alias the same as the namespace name with the leading parts removed.

```clojure
(ns com.example.application
  (:require
   [clojure.java.io :as io]
   [clojure.string :as string]))
```

Keep enough trailing parts to make each alias unique. Did you know that namespace aliases can have dots in them?

```clojure
[clojure.data.xml :as data.xml]
[clojure.xml :as xml]
```

Eliminate redundant words such as “core” and “clj” in aliases.

```clojure
[clj-http :as http]
[clj-time.core :as time]
[clj-time.format :as time.format]
```

Use :refer sparingly. It’s good for symbols that have no alphabetic characters, such as >! <! >!! <!! in core.async, or heavily-used macros such those in clojure.test.

You can combine :refer and :as in the same :require clause.

```clojure
[clojure.core.async :as async :refer [<! >! <!! >!!]]
[clojure.test :refer [deftest is]]
```

