# Clojure[script] automatically generated live documentation.


[codox](https://github.com/weavejester/codox) is a great tool for generating API documentation from Clojure or ClojureScript source code.

With the klipse theme for codox, you can make your `codox` documentation live and interactive powered by the 
[KLIPSE plugin](https://github.com/viebel/klipse).

In addition to **words** that describe your functions, now you can add **interactive code examples**. It will be much easier for your users to understand how to use your functions.

![Gadjett](https://github.com/viebel/codox-klipse-theme/raw/master/gadjett.gif)


## Usage

The prerequesite is that your library is either:

- A clojurescript library
- A portable clojure library that uses reader conditionals

Add `codox-klipse-theme` to your `dev-dependencies` in `project.clj`:

[![Clojars Project](https://img.shields.io/clojars/v/viebel/codox-klipse-theme.svg)](https://clojars.org/viebel/codox-klipse-theme)

And add the following settings to the `:codox` map.


Lets' say your github user is `my_user` and your repo is `my_repo`, 
with namespace `my_repo.my_ns` with a function `my_func`, then add 
the following options to `:codox`:

```clojure
 :codox {:metadata {:doc/format :markdown}
         :output-path "docs"
         :themes [:default [:klipse
         {:klipse/external-libs
         "https://raw.githubusercontent.com/my_user/my_repo/master/src/"
          :klipse/require-statement
          "(ns my.test
          (:require [my_repo.my_ns :as my_ns :refer [my_func]]))"]]
         }
```

And in the docstring of `my_ns/my_func`, add the code examples like this:

    ~~~klipse
    (my_func 1 2 3)
    ~~~

(`~~~klipse` creates a code block with `class="klipse"`.)

## Explanations

- The `:klipse/external-libs` option lets KLIPSE know where to look for your source code in order to evaluate it in the browser.
- The `:klipse/require-statement` option contains a string to create the namespace where the code from your documentation is going to run. KLIPSE will create a hidden code snippet with this `require` code.
- It is not mandatory to use `:markdown` but it is much easier. Feel free to open an issue if you need support for other formats.

## Example

Take a look at how it is done in the gadjett library:

- The [project.clj file](https://github.com/viebel/gadjett/blob/master/project.clj#L16-L25)
- The [live documentation](http://viebel.github.io/gadjett/gadjett.collections.html)
