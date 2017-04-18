# Clojure[script] automatically generated interactive documentation.


[codox](https://github.com/weavejester/codox) is a great tool for generating API documentation from Clojure or ClojureScript source code.

With the klipse theme for codox, you can make your `codox` documentation live and interactive powered by the 
[KLIPSE plugin](https://github.com/viebel/klipse).

In addition to **words** that describe your functions, now you can add **interactive code examples**. It will be much easier for your users to understand how to use your functions.

![Gadjett](https://github.com/viebel/codox-klipse-theme/raw/master/gadjett.gif)


## Usage

You must use `codox` version above `0.10.3`.


The prerequesite is that your library is either:

- A clojurescript library
- A portable clojure library that uses reader conditionals


Add `codox` and `codox-klipse-theme` to your `dev-dependencies` in `project.clj`:

[![Clojars Project](https://img.shields.io/clojars/v/viebel/codox-klipse-theme.svg)](https://clojars.org/viebel/codox-klipse-theme)

```clojure
{:dependencies [[viebel/codox-klipse-theme "0.0.5"]]
 :plugins [[lein-codox "0.10.3"]]}
```

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
          (:require [my_repo.my_ns :as my_ns :refer [my_func]]))"}]]
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


## Performances and caching

Retrieving the sources of your lib at run-time slows down the documentation page load - especially if your lib creates a lot of macros.

A solution to that is to cache your namespaces.

Here is how it works:

#### 1. create the cached namespaces

Here is how to create the cached namespaces for `my_repo.my_ns` with `lumo` and store them under `docs/cache-cljs`

```bash
lumo -k docs/cache-cljs -c`lein classpath` -e "(require 'my_repo.my_ns)"
```
Be patient: It might take a couple of seconds to generate all the cached namespaces....

#### 1. configure the cached namespaces in klipse

Now you need to tell klipse what namespaces are cached and what is the cached location (instead of `:klipse/external-libs`:

```clojure
 :codox {:metadata {:doc/format :markdown}
         :output-path "docs"
         :themes [:default [:klipse
         {
         :klipse/cached-macro-ns-regexp #"/my_repo\..*/" ; pay attention the regexp is expressed as a string wrapped in //
         :klipse/cached-ns-regexp #"/my_repo\..*/" ; pay attention the regexp is expressed as a string wrapped in //
         :klipse/bundled-ns-ignore-regexp #"/my_repo\..*/" ; pay attention the regexp is expressed as a string wrapped in //		 
         :klipse/cached-ns-root "./cache-cljs"
         :klipse/require-statement
          "(ns my.test
          (:require [my_repo.my_ns :as my_ns :refer [my_func]]))"]]
         }
```

The reason we use regexps is that your lib might depend on other libs that are also cached. In this case you will need have:

```clojure
         :klipse/cached-ns-regexp #"/my_repo\..*/|other_repo\..*"
```

