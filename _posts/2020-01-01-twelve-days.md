---

title: Twelve days of Christmas

layout: post

---

A fun software challenge has been floating around my social network this holiday season: given a list of gifts, print out the lyrics to _The Twelve Days of Christmas_. I've been itching to write Clojure again lately, and so I decided to use this as an opportunity to get my head back in that world a bit. I wrote the code using a REPL, because that's a big part of my enjoyment of Clojure.

There are some interesting bits of the challenge that I wanted to solve in a clever way. First, the song uses both cardinal numbers (one, two, three) and ordinal numbers (first, second, third). The simplest way to implement this is to just list out the first twelve cardinals and first twelve ordinals, but I thought it would be fun to use Clojure's dynamism to pull in a dependency from the Leiningen REPL.

To do that, I first needed to add the `lein-exec` plugin to my `~/.lein/profiles.clj`, which gives access to Leiningen's pomegranate library.

```clj
{:user {:plugins [[lein-exec "0.3.4"]]}}
```

This gives me access to `deps`, a function that lets me pull dependencies down from Maven Central. There's a package by IBM called `icu4j` which has features for printing cardinal and ordinal numbers:

```clj
(use '[leiningen.exec :only (deps)])
(deps '[[com.ibm.icu/icu4j "65.1"]])
(import 'java.util.Locale 'com.ibm.icu.text.RuleBasedNumberFormat)

;; set up fns for formatting numbers
(def rbnf (RuleBasedNumberFormat. Locale/ENGLISH RuleBasedNumberFormat/SPELLOUT))
(defn cardinal [n] (.format rbnf n "%spellout-cardinal"))
(defn ordinal [n] (.format rbnf (long n) "%spellout-ordinal"))
```

I now have `cardinal` and `ordinal` functions, which can be called with a number to get the cardinal/ordinal string of that number.

Next, I need to render a list of gifts using cardinal numbers-four calling birds, three French hens, etc-and decided to use Clojure's dispatch-on-arity and recursion to print out all the gifts passed to it. A multimethod may be more appropriate here, using the length of the passed vector rather than using arity like this. Nevertheless:

```clj
(defn gift-lines
  ([gift] (str "  a " gift)) ;; fn for one gift (used on the first day)
  ([gift-1 gift-2]           ;; fn for two gifts (2nd day and later)
    (str "  " (cardinal 2) " " gift-1 "\n  and a " gift-2))
  ([gift-1 gift-2 & gifts]   ;; fn for many gifts (3rd day and later)
    (str "  "
      (cardinal (+ 2 (count gifts))) " " gift-1 "\n"
      (apply gift-lines gift-2 gifts))))
```

Now, a function that renders a whole verse, which is just adding a prefix of "On the [ordinal] day of Christmas..." to the previous function, then rendering the list of gifts in reverse.

```clj
(defn verse [gifts] ;; generate a verse (day) of the song
  (str
    "On the " (ordinal (count gifts)) " day of Christmas, my true love gave to me\n"
    (apply gift-lines (reverse gifts))))
```

And lastly, a function that prints out the whole song, which is a loop that consumes progressively more and more of the list of gifts until it runs out. I could call this function with any number or any list of gifts, and it should absorb the difference gracefully.

```clj
(defn song [gifts] ;; print out all verses of the song for the list of gifts
  (doseq [day (range 1 (inc (count gifts)))]
    (println (verse (take day gifts)))
    (println "")))
```

Calling the `song` function:

```clj
(song [
  "partridge in a pear tree"
  "turtle doves"
  "French hens"
  "calling birds"
  "gold rings" 
  "geese a-laying" 
  "swans a-swimming"
  "maids a-milking"
  "ladies dancing"
  "lords a-leaping"
  "pipers piping"
  "drummers drumming"])
```

Outputs:

```
On the first day of Christmas, my true love gave to me
  a partridge in a pear tree
On the second day of Christmas, my true love gave to me
  two turtle doves
  and a partridge in a pear tree
...
On the twelfth day of Christmas, my true love gave to me
  twelve drummers drumming
  eleven pipers piping
  ten lords a-leaping
  nine ladies dancing
  eight maids a-milking
  seven swans a-swimming
  six geese a-laying
  five gold rings
  four calling birds
  three French hens
  two turtle doves
  and a partridge in a pear tree
```

A fun little challenge, and a fun thing to do with a Clojure REPL.