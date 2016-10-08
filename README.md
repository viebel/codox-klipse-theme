# codox-klipse-theme
Klipse theme for codox

## Live Documentation

You can make your documentation live and interactive with the 
[KLIPSE plugin](https://github.com/viebel/klipse), by including the 
`:klipse` theme in your `project.clj`.

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

Take a look at how it is done in the
[gadjett library](https://github.com/viebel/gadjett).
