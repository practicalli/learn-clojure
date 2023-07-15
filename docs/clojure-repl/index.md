# Clojure REPL

[:fontawesome-solid-book-open: Install Clojure CLI and Practicalli Clojure CLI Config](https://practical.li/clojure/install/){target=_blank} for a comprehensive set of developmet tools.

Use a [:fontawesome-solid-book-open: terminal UI REPL](#terminal-ui-repl) as a quick way to get started, or set up a preferred [:fontawesome-solid-book-open: Clojure editor](#clojure-editors).

??? HINT "Editor Connected REPL"
    An [:fontawesome-solid-book-open: Editor connected REPL](https://practical.li/clojure/clojure-editors/) is recommended once working with Clojure projects

!!! HINT "Create a Clojure project from a template"
    `:project/create` alias from [:fontawesome-solid-book-open: Practicalli Clojure CLI Config](https://practical.li/clojure/clojure-cli/practicalli-config/) will create a Clojure project structure
     ```clojure
     clojure -T:project/create :name github-name/project-name
     ```

## Terminal UI REPL

Rebel Readline provides a rich REPL experience, providing syntax highlighting, function signatures and documentation.

Start Rebel Readline REPL using the `:repl/rebel` alias provided by Practicalli Clojure CLI Config

```shell
clojure -M:repl/rebel
```

[:fontawesome-solid-book-open: Rebel REPL Terminal UI](https://practical.li/clojure/clojure-cli/repl/){target=_blank .md-button}

<p style="text-align:center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/U19TWMsg0s0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>


## Clojure Editors

Clojure editors are the preferred way to write code and evaluating source code.  Working with source files is more effective than entering all expressions directly at a REPL prompt.

Use an editor to **jack-in** (start) a Clojure REPL process and connect to the running REPL.

Or **connect** to a running REPL process, e.g. Rebel Terminal UI (over a network repl, nREPL).

Use an editor that is most familiar or comfortable to minimise the learning curve.

Clojure editors should provide

- running / connecting to a REPL process
- evaluation results inline (instant feedback on code behaviour)
- syntax highlighting, including parens matching
- Structural editing, balancing parens (paredit / parinfer)
- data inspector to navigate large & nested data, or connection to external [:fontawesome-solid-book-open: data inpector tools](https://practical.li/clojure/data-inspector/)

[:fontawesome-solid-book-open: Clojure aware editors](https://practical.li/clojure/clojure-editors/){target=_blank .md-button}

![:fontawesome-solid-book-open: Clojure Editor connected REPL](https://raw.githubusercontent.com/practicalli/graphic-design/live/clojure/clojure-repl-driven-development-clojure-aware-editor.png){loading=lazy}
