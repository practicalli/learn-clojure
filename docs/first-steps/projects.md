# Clojure projects

A Clojure CLI project contains a `deps.edn` configuration file, containing a hash-map that defines the fundamentals of a project.

!!! HINT "Create project from template"
    Create a minimal project using the `:project/create` alias
    ```shell
    clojure -T:project/create 
    ```
    Pass `:name practicalli/project-name` to define a specific name for the project.

    `:project/create` from [:fontawesome-solid-book-open: Practicalli Clojure CLI config](https://practical.li/clojure/clojure-cli/practicalli-config/){target=_blank} uses deps-new tool to create a project from a template

    [:fontawesome-solid-book-open: Practicalli Project Templates](https://practical.li/clojure/clojure-cli/projects/templates/){target=_blank} creates a project with an effective REPL workflow tools. 


## Anatomy of a project

`deps.edn` a declarative definition of source paths, library dependencies and aliases to support development tools.

`build.clj` a programatic approach to building jars and uberjars and related build tasks, using org.[:fontawesome-solid-book-open: clojure/tools.build library](https://practical.li/clojure/clojure-cli/projects/package/tools-build/){target=_blank} .

`resources` directory containing EDN configuration files and any other artifacts to ship with the Clojure project, e.g. images, CSS, JavaScript for web services.

`src` directory containing a tree of source code, typically including the organisation as the first directory under src, e.g `practicalli`, `org/clojure` or `io.seancorfield`

`test` directory containing unit test code in a tree that mirrors the `src` directory structure.


## deps.edn structure

The `deps.edn` file is defined by a hash-map, {}, with top-level keys: `:paths`, `:deps`, `:alias`, `:mvn/repositories`, `:mvn/local`

`:paths` location of Clojure source code and Edn configuration, adding locations to the class path.

`:deps` library dependencies to build the Clojure project, added to the class path.

`:aliases` tools and libraries optionally included during development to support the local workflow (repl & testing tools, data inspectors, reload repl state, etc.)

`:mvn/repositories` - URLs for Maven & Clojars repositories included in Clojure CLI by default, mirrors and depenency caches are typically added to the user configuration.

`:mvn/local-repo` - set local repository path, default `$HOME/.m2/repository`

??? HINT "Migrate from Leiningen"
    Adding a `deps.edn` file to use Clojure CLI with an existing Leiningen project.  No changes to the source code or test code should be required (unless using a Leinignen plugin that modifys code, e.g lein ring)
    Leiningen uses the `project.clj` configuration for projects, a mostly declarative approach.

    Create a `deps.edn` configuration that includes the main paths and deps from the `project.clj` file.

    Include aliases that support the development workflow


### Aliases

The `deps.edn` top level `:aliases` key is associated with a hash-map, `{}`, containing zero or more alias definitions.

An alias definition can use any of the following keys

`:extra-paths` to add libraries to the class path, e.g. `:paths ["test"]` to add the `test` directory

`:extra-deps` to add libraries to the class path, using the same form as `:deps`

`:main-opts` to pass a vector of string arguments when running code via `clojure.main`

`:exec-fn` to specify a fully-qualified function to run via `clojure.exec`

`:exec-opts` to pass a hash-map of arguments when running code via `clojure.exec`

`:replace-paths` to add only the paths specified to the class path 

`:replace-deps` to add only the libraries specificed to the class path 


```clojure
  :repl/rebel
  {:extra-deps {nrepl/nrepl                {:mvn/version "1.0.0"}
                cider/cider-nrepl          {:mvn/version "0.31.0"}
                com.bhauman/rebel-readline {:mvn/version "0.1.4"}}
   :main-opts  ["-e" "(apply require clojure.main/repl-requires)"
                "--main" "nrepl.cmdline"
                "--middleware" "[cider.nrepl/cider-middleware]"
                "--interactive"
                "-f" "rebel-readline.main/-main"]}
```

> Inside an alias definition, `:deps` is an alias for `replace-deps` and `:paths`
is an alias for `:replace-paths`.  Practicalli recommends using the explicit `:replace-*` keys to avoid ambiguaty.


??? HINT "Aliases provided by Practicalli Clojure CLI Config"
    A wide range of aliases to support development tools and reloaded REPL workflow are provided by [:fontawesome-solid-book-open: Practicalli Clojure CLI Config](https://practical.li/clojure/clojure-cli/practicalli-config/){target=_blank}


## Namespace structure

Each Clojure file in either `src` or `test` should define the namespace name using the `ns` form. 

```clojure title="src/practicalli/gameboard/service.clj"
(ns practicalli.gameboard.service)
```

The `ns` for can also require libraries used by the code within the namespace.  Libraries are added using a short meaningful alias using the `:as` directive.


```clojure title="src/practicalli/gameboard/service.clj"
(ns practicalli.gameboard.service
  (:require 
   [com.brunobiachi.mulog :as mulog])

(mulog/set-global-context! 
 {:app-name "Practicalli Gameboard API Service",
  :version "0.1.0" 
  :env "dev"})
```

When a namespace requires a library for a specific purpose, then function names can be required directly

The purpose of the `-test` namespaces under the `test` directory is to run unit tests, so functions from `clojure.test` can be required directly using the `:refer` directive.

```clojure title="test/practicalli/gameboard/service_test.clj"
(ns practicalli.gameboard.service-test
  (:require 
   [clojure.test :refer [deftest is testing]]
   [practicalli.gameboard.service :as service])

(deftest
  (testing "Gameboard API Service startup tests"),
    (is (seq? service/running-systemg)))
```

!!! CLOJURE-IDIOM "Only require using alias or refer specific functions"
    The `(:use namespace.name)` directive and `(:require namespace.name :refer :all)` are strongly discouraged as they include the whole library and introduce abiguaty and potential conflict.

    
