---

title: Getting an array of "characters" in Clojure

layout: post

---

I'm continuing to noodle around in Clojure and needed to split a string into its component characters. If you know your string will be plain-old-ASCII, you can do:

```clj
(clojure.string/split "hello" #"")
;; ["h" "e" "l" "l" "o"]
```

But this gets funny if you want to split up a string that has emoji in it:

```clj
(def ghosties "👻👻👻")
(clojure.string/split ghosties #"")
;; ["?" "?" "?" "?" "?" "?"]
```

This is because emoji are represented by two bytes under the hood, called a *surrogate pair*. When you split up a string using the empty regex, it spits it on each byte not on each character. [This is a problem in Javascript as well.][1]

To solve this problem in Clojure, we need to reach into the Java APIs for manipulating strings and characters:

```clj
;; For a given integer codepoint, return the string representation of it
(defn codepoint-to-str [cpt]
  (-> (StringBuilder.)
      (.appendCodePoint cpt)
      .toString))

;; For a given string, split it into codepoints and generate a string
;; for each individual code point.
(defn to-string-array [s]
    (map
      codepoint-to-str 
      (iterator-seq (.iterator (.codePoints s)))))

(to-string-array ghosties)
;; ("👻" "👻" "👻")
```



[1]: https://twitter.com/TeslaNick/status/1206942190292938753