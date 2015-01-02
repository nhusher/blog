
The description of an idea is a user interface for thinking about it. The more complexity it takes to describe an idea, the more difficult it is to reason about it abstractly. Here's a fairly simple problem expressed by a great Persian mathematician al-Khwārizmī from the tenth century:<sup>[1]</sup>

> What is the square which when taken with ten of its roots will give a sum of thirty nine? Now the roots in the problem before us are ten. Therefore take five, which multiplied by itself gives twenty five, and amount you add to thirty nine to give sixty four. Having taken the square root of this which is eight, subtract from this half the roots, five leaving three. The number three represents one root of this square, which itself, of course, is nine. Nine therefore gives the square.

The above passage is extremely difficult to understand. A senior in high school might now represent this problem and its solution as:

    x^2 + 10x = 39
    x^2 - 3x + 13x - 39 = 0
    (x+13) * (x-3) = 0
    x = 3
    x = -13

Or, more visually:

GRAPH IMAGE

A paragraph of text was replaced by a symbolic notation that makes the problem easier to understand at a glance. The graph allows us to represent the nature of the formula visually, which might show features that weren't obvious.

More practically, it would be unimaginable to derive higher-order mathematical concepts like calculus from the prose above. It's telling that calculus was derived less than 20 years after the "completion" of the basic symbolic notation of algebra.<sup>[2]</sup>




#### A brief digression about semantics.

The distinction between a language's syntax and it's semantics is an important one. Syntax is the arrangement of symbols that make up a language; semantics is the meaning of those arrangements. We tend to think about programming languages as variations of *syntax*&mdash;that the primary differences are in the arrangement of symbols&mdash;and not a lot about the differences in the *semantics*, or the underlying meaning of symbols. 

I believe this is because most programming languages a developer will encounter have very similar semantics, and differ chiefly in how they express those ideas through syntax. For example, Objective-C is considered to be a weird language, even though it's a C-based, object-oriented, has classical inheritance, procedural, and memory managed. A big reason it's considered weird is because it uses a lot of square brackets and has funny function names.

But even large variations in syntax are actually easy to overcome: If you know Java, it's pretty easy to learn Groovy; If you know Groovy; it's pretty easy to learn Javascript. By contrast, it can be difficult to jump to a conceptually-alien language.

Clojure has both of these problems: it has a relatively unfamiliar syntax and a lot of tricky concepts to master to be proficient in it. At the same time that you're wrestling with the nuances of S-expressions (the fundamental sytnax of a Clojure program), you also have to work with functional programming, immutable data, and a sophisticated standard library.

#### Why learn Clojure?

I believe there is value in learning new languages with unfamiliar ideas. Every programming language that's made it to production has been a successful attempt at solving a problem in software. The ideas encoded in these languages are often portable to other languages, or can be an introduction into new ways of thinking about a particular set of problems. I believe that the concepts embedded in Clojure are powerful enough that they will make you a significantly better better programmer in whatever language you choose to program in.<sup>[1]</sup> What are these concepts?

1. **Functional programming "all the way down."** Your primary tool for building programs in Clojure are function.<sup>[2]</sup> Even constructs we would generally consider to be an expression (like adding two numbers together) or language construct (like creating "classes" or "objects") are functions.

    For example, it's 


2. **Immutability everywhere.** Everything in Clojure is an immutable value, which simplifies a lot of problems associated with asynchronous or concurrent programming.

3. **Interactivity built-in.** It's much easier to explore new ideas or fix issues if you can work directly with your application as it's running. There's far less separation between between the "writing" and "running" phases of a Clojure app.

Before I dive into the details of semantics, I think it's worthwhile to do a sprinting tour of syntax. Why? Because a very common response to Clojure syntax is, "it looks like a cool language, but I can't make heads nor tails of the syntax."

Here's a simple and representative bit of clojure code:

```
(defrecord Webserver [router port server]
  component/Lifecycle
  (start [component]
    (let [jetty (run-jetty (:handler router) { :port port, :join? false })]
      (prn "Started web server on port" port)
      (assoc component :server jetty)))

  (stop [component]
    (prn "Stopping jetty")
    (.stop server)
    (assoc component :server nil)))


(defn make-webserver
  ([port] (map->Webserver { :port port }))
  ([]     (map->Webserver { :port 8080 })))

```

The absolute first thing to know is that Clojure's syntax gives you a lot of freedom in naming things. You are encouraged to use angle brackets, dashes, asterisks, question marks, and exclamation points and so forth. These characters are not reserved by the programming language. This results in very expressive naming conventions: A converter function might be named `json->model`, a dynamic value usually has "earmuffs" (e.g. `*app-state*`) or if you want to know if a string is empty, you call `blank?`. This flexibility gives you a lot of power to describe your own domain-specific languages inside of Clojure when you have a need for them.

Clojure programs are described entirely with data structure literals. Clojure has ten or so to describe the data and program you're working with. Here are the most common, almost all of which should be familiar from Javascript:

* `true`, `false`, `nil` - The values true, false, and *nothing* are pretty simple 
 
* `1000` - Numbers look the way you'd expect <sup>[3]</sup>

* `"Hello world"` - So do strings

* `:nietzsche` - This is a new one. This is called a keyword, and it's a placeholder value that always means itself. It's similar to the Symbol type in Ruby, or a Java Enum value. It provides fast equality checking and has a few other uses throughout the language.

* `[ :oranges, :bananas, :pineapples ]` - This is a *vector* of keywords. A vector is basically an array.

* `{ :name "Rich Hickey", :employer "Cognitect" }` - This is a map. Note that the keys here are *keywords*, which aren't strings. Maps in Clojure are like Java-style maps, in that the keys can be of any type.

* `#{ :oranges, :bananas }` - This is a set. It's like a vector, but has only unique items.

And that's basically it. Not very different from Javascript.

Ok, let's move on to actually creating some new stuff. An expression in Clojure is contained by parenthesis, and the first symbol within the parenthesis is a function name. It looks like this:

```
(inc 9)  ; returns the number 10
```

Where `inc` is a function that increments the value `9`. Expressions in Clojure are also a kind of data structure literal, called *lists*.<sup>[4]</sup> I left that out because they aren't often used by programmers describe data, but this fact makes it very easy to perform source code transformations on Clojure programs.


<ol class="footnotes">
  <li id="f2-1">
    This example is lifted from Bret Victor's lecture, <em><a href="http://worrydream.com/MediaForThinkingTheUnthinkable/">Media for Thinking the Unthinkable</a></em>. It's an excellent talk on the nature of problem solving and building new affordances for our weak monkey brains.
  </li>
  <li id="f2-2">
    By "complete," I mean that the full set of symbols &equals;, &plus;, &minus;, &divide;, &times;, as well as notations for exponents, roots, grouping, and so forth. The division symbol was the last to be introduced in 1659, according to <a href="http://en.wikipedia.org/wiki/Table_of_mathematical_symbols_by_introduction_date">this awesome table I found on Wikipedia</a>. These operations existed for centuries before their symbols were commonly used. I don't think it's not a great leap to assume that innovations in notation drove the discovery of calculus, and not the other way around. 
  </li>
  <li id="f2-1">
    Clojure isn't the only language with unfamiliar concepts, but it is a great example. Haskell, Prolog, and even OOP languages like Eiffel have powerful and alien concepts built into them.
  </li>
  <li id="f2-2">
    Some Clojure "functions" are actually special forms, in that they're semi-magical and generally depend on doing things inside the parent runtime (either the JVM or javascript). Nevertheless, for most intents and purposes they behave like functions.  
  </li>

  <li id="f2-4">
    It's of academic interest that Clojure is a considered by most to be flavor of a language called Lisp, the name of which comes from "List Processing." Standard Lisp only has list literals  and has some greater flexibility in how symbols are processed.
  </li>
</ol>

[1]: #f2-1
[2]: #f2-2
[4]: #f2-4



http://rationale.austhink.com/rationale2.0/ib/exercises/tok/syntax_semantics.htm
