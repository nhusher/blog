---

title: Clojure for the Javascript Programmer

kerned_title: Clojure for the<br />Javascript Programmer

layout: post

---

*What follows is the first entry in what I imagine will be a short series about Clojure, all from the perspective of a someone who writes a lot of Javascript at his day job. This is intended to be a simplified introduction to Clojure. It will become a short presentation to interested engineers [where I work](http://dealer.com). I've deliberately simplified and omitted some detail that would be covered by a more-comprehensive resource.*

I've become a bit infamous among my peers for evangelizing Clojure and the unfamiliar concepts it makes use of. I'm especially interested in ideas that can be ported to more-familiar languages because I believe they make it easier to think about complex software. I believe the easiest way to learn those concepts is to become at least passingly familiar with Clojure.

Clojure can be an intimidating beast from a distance, but up-close it's not nearly as terrifying. A recent comment by a coworker is summarized by, "Clojure looks really powerful, but when I look at that code I'm totally lost." I believe this sentiment is rooted in Clojure's unfamiliar syntax, but the language described by that syntax isn't very much different from Javascript. Brenden Eich based a lot of Javascript on Smalltalk, which was itself inspired by Lisp, and Clojure is a Lisp dialect. So, while the syntaxes of each language might be wildly different, many of the underlying ideas are the same.

### A quick language primer.

There are two rules for translating between Javascript and Clojure. The first rule is that parenthesis begin to the left of the function name:

```
capitalize("hello world")  // => "Hello World"
(capitalize "hello world") ;; => "Hello World"
```

You can see that the function named `capitalize` is enclosed by parenthesis, rather than followed by them. The second rule is that everything is a functional expression. Even mathematical operators are defined as functions, and are enclosed by parenthesis:

```
2 * 4         // => 8
(* 4 2)       ;; => 8

2 * 4 + 1     // => 9
(+ (* 2 4) 1) ;; => 9
```

This looks awkward at first, but has the advantage of never being ambiguous about operator ordering; the operands to addition are enclosed by parenthesis, and any sub-expressions are evaluated prior to evaluating the addition.

Clojure has a bunch of built-in literals to describe different types of data, almost all of them should be familiar from Javascript:

* `true`, `false`, `nil` - The values true, false, and *nothing* are pretty simple 
* `1000`, `1.25` - Numbers look the way you'd expect <sup>[1]</sup>
* `"Hello world"` - So do strings
* `:nietzsche` - This is a new one. This is called a keyword, and it's a placeholder value that always means itself. It's similar to the Symbol type in Ruby, or a Java Enum value. It provides fast equality checking and has a few other uses throughout the language.
* `[ :oranges, :bananas, :pineapples ]` - This is a *vector* of keywords. A vector is basically an array.
* `{ :name "Rich Hickey", :employer "Cognitect" }` - This is a map. Note that the keys here are *keywords*, which aren't strings. Maps in Clojure are like Java-style maps, in that the keys can be of any type.
* `#{ :oranges, :bananas }` - This is a set. It's like a vector, but has only unique items.
* `(fibonacci 5)`

### Why learn Clojure?

"Clojure feels like a general-purpose language beamed back from the near future." This is how the book *Programming Clojure* describes the language. Clojure has a constellation of features that will help you build better programs, but remains pragmatic about how those programs need to be written. If you need to fall back to an imperative or object-oriented style, Clojure will not stop you.

#### 1. Write less code.

The most striking feature to newcomers to the language is how expressive it is. A Clojure program can easily be half as long as the equivalent in other languages, and often even smaller than that. Once you're comfortable with the standard library, it will be hard to go back to more-verbose languages. Here are two snippets in Javascript that check if an array contains only numbers:

```javascript
// Naive Javascript:
function areAllNumbers(collection) {
    for(var i = 0; i < collection.length; i++) {
        if(typeof collection[i] !== 'number') {
            return false;
        }
    }
    return true;
}

// Or, using Javascript's collection methods:
collection.some(function(item) { return typeof item === 'number'; });
```

This is the in Clojure&mdash;even if you're unfamiliar with the language, it should be possible to see the "bones" of the shared algorithm:

```clojure
;; Naive Clojure:
(defn all-numbers? [coll]    
  (cond
    (empty? coll) true
    (number? (first coll)) (recur (rest coll))
    :else false))

;; And using the Clojure collections library:
(every? number? collection)
```
    
Javascript stands out as a very expressive language, and yet the Clojure samples remain significantly smaller. This disparity is only magnified as programs become more complex.

There is an important distinction between *conciseness* and *terseness*. Minification or creative "code golf" can make the source code smaller, but at the cost of understandibility. This is what I mean by terse. Conciseness is about being able to express a complete idea simply in a small space in a way that allows it to compose into more complex abstractions.

#### 2. Think Functionally.

The next obvious feature is that it's a functional language. This is a really weird thing to get used to, because a lot of Javascript is calling functions that are attached to objects to mutate their state. Instead, in Clojure, you call functions against values to get a new result. If you want to append a value to an array in javascript, you use the `concat` method with the value you want to add, in Clojure you call the `conj` (short for "conjoin") with the collection and the value you want to add.<sup>[2]</sup>

```
[1, 2, 3].concat(4); // => [1, 2, 3, 4]
(conj [1, 2, 3] 4)   ;; => [1, 2, 3, 4]
```

This is also a good time to bring up the excellent standard library Clojure has. Rich Hickey, the author of Clojure, is allergic to re-implementing the same functions for different data types, and the standard library reflects this. The standard library is large, but can be used across many data types. The biggest benefaciaries of this approach are collections. Every collection (including strings and maps) in the language can use the functions that operate on sequences. The values of the collections are intelligably coerced into a shared representation that can be easily acted upon:

```
(first [ 1 2 3 ])         ;; => [ 1 ]
(first { :foo 1 :bar 2 }) ;; => [ :foo 1 ]
(first "Hello")           ;; => \H
(first #{ :red :green })  ;; => :red
```

No `Object.keys(o).map(...)`&mdash;you can map directly over the object in a sane fashion.

#### 3. Reject mutability.

I believe most-important feature of clojure is immutability.

````
function myUserProcessor(user) {
    if(user.somethingIsTrue) {
        getSomethingFromServer(user).then(function() {
            
        });
}

````



#### 4. All this other great stuff!

* Live coding
* Managed CSP
* Homoiconic code
* Macros
* ClojureScript
* Easy testability
* JVM / Javascript interop
* Value destructuring
* Ad-hoc Polymorphism
* Lazy and Infinite Sequences


### Clojure will make you a better programmer.

And if you go down that road, there's nothing you can do about it.



<ol class="footnotes">
  <li id="f2-1">
    Clojure numbers also support a <em>rational</em> form. So if you divide 2 by 3, you'll get a rational value of &frac23; rather than the less-accurate 0.66&#773;. ClojureScript does not have rational numbers and use the Javascript numbers instead.
  </li>
  <li id="f2-2">
    The common function to use in Javascript would actually be <code>push</code>, but <code>push</code> and <code>conj</code> aren't strictly equivalent. <code>push</code> alters the array in place and returns the value you pushed into it. <code>concat</code> combines the values you give it into a new array and returns that value. As a style note, I tend to avoid <code>push</code> unless it makes my code significantly more complicated.
  </li>
</ol>

[1]: #f2-1
[2]: #f2-2


