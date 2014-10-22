---

title: Clojure for the Javascript Programmer

kerned_title: Clojure for the<br />Javascript Programmer

layout: post

---

*What follows is the first entry in what I imagine will be a short series about Clojure, all from the perspective of a someone who writes a lot of Javascript at his day job. This is intended to be a simplified introduction to Clojure. It will become a short presentation to interested engineers [where I work](http://dealer.com). I've deliberately simplified and omitted some detail that would be covered by a more-comprehensive resource.*

I've become a bit infamous among my peers for evangelizing Clojure and the unfamiliar concepts it makes use of. I'm especially interested in ideas that can be ported to more-familiar languages because I believe they make it easier to think about complex software. I believe the easiest way to learn those concepts is to become at least passingly familiar with Clojure.

Clojure can be an intimidating beast from a distance, but up-close it's not nearly as terrifying. A recent comment by a coworker is summarized by, "Clojure seems really powerful, but when I look at that code I'm totally lost." I believe this sentiment is rooted in Clojure's unfamiliar syntax, but the language described by that syntax isn't very much different from Javascript. Brenden Eich based a lot of Javascript on Smalltalk, which was itself inspired by Lisp. Clojure is a Lisp dialect, so the two languages are distant cousins of one another; while the syntaxes of each language might be wildly different, many of the underlying ideas are the same.

### A quick language primer.

There are two rules for translating between Javascript and Clojure. The first rule is that parenthesis begin to the left of the function name:

```
capitalize("hello world")  // => "Hello World"
(capitalize "hello world") ;; => "Hello World"
```

You can see that the function named `capitalize` is enclosed by parenthesis, rather than followed by them.

The second rule is that everything is a functional expression. Even mathematical operators are defined as functions, and are enclosed by parenthesis:

```
2 * 4         // => 8
(* 4 2)       ;; => 8

2 * 4 + 1     // => 9
(+ (* 2 4) 1) ;; => 9
```

This looks awkward at first, but has the advantage of never being ambiguous about operator ordering.

Clojure has a bunch of built-in literals to describe different types of data, almost all of them should be familiar from Javascript:

* `true`, `false`, `nil` - The values true, false, and *nothing* are pretty simple 
* `1000`, `1.25` - Numbers look the way you'd expect <sup>[1]</sup>
* `"Hello world"` - So do strings
* `:nietzsche` - This is a new one. This is called a keyword, and it's a placeholder value that always means itself. It's similar to the Symbol type in Ruby, or a Java Enum value. It provides fast equality checking and has a few other uses throughout the language.
* `[ :oranges, :bananas, :pineapples ]` - This is a *vector* of keywords. A vector is basically an array.
* `{ :name "Rich Hickey", :employer "Cognitect" }` - This is a map. Note that the keys here are *keywords*, which aren't strings. Maps in Clojure are like Java-style maps, in that the keys can be of any type.
* `#{ :oranges, :bananas }` - This is a set. It's like a vector, but has only unique items.
* `(fibonacci 5)`

The last peculiar thing about Clojure is that it doesn't reserve very many characters for syntax purposes. The following are all valid names in Clojure: `number?`, `transact!`, `assoc-in`, `clj->json`, `*app-state*`,  `>!`.


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
collection.every(function(item) { return typeof item === 'number'; });
```

The following examples are the same algorithms in Clojure; Even if you're unfamiliar with the language, it should be possible to see the "bones" of what's going on:

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

There is an important distinction between *conciseness* and *terseness*. Minification or creative "code golf" can make the source code smaller, but at the cost of understandability. This is what I mean by terse. Conciseness is about being able to express an idea intelligibly in a small space, such that it can be composed easily into higher-level concepts.

#### 2. Think Functionally.

The next obvious feature is that it's a functional language, which can be a tough thing to get used to. Much of Javascript is calling functions attached to objects to produce a new state. In Clojure, you call functions against values to produce a new value. If you want to append to an array in Javascript, you use the `concat` method with the value you want to add, in Clojure you call the `conj` (short for "conjoin") with the collection and the value you want to add.<sup>[2]</sup>

```
[1, 2, 3].concat(4); // => [1, 2, 3, 4]
(conj [1, 2, 3] 4)   ;; => [1, 2, 3, 4]
```

This is also a good time to bring up the excellent standard library Clojure has. Rich Hickey, the author of Clojure, is allergic to re-implementing the same functions for different data types, and the standard library reflects this.

The Clojure standard library is large, but can be used across many data types. The biggest beneficiaries of this approach are collections. Every collection in the language can use the functions that operate on sequences (including strings and maps). The values of the collections are intelligibly coerced into a shared representation that can be easily acted upon:

```clojure
(first [ 1 2 3 ])         ;; => [ 1 ]
(first { :foo 1 :bar 2 }) ;; => [ :foo 1 ]
(first "Hello")           ;; => \H
(first #{ :red :green })  ;; => :red
```


#### 3. Embrace Immutability.

I believe most-important feature of Clojure to understand is immutability. In Javascript, strings are immutable&mdash;there's nothing you can do to a string that won't create a new string as a result. If you call `.toUpperCase()` on a string, it will return a *new copy* of that string with all the letters in upper-case, rather than modifying the string in place. In Clojure, all data behaves in this way; If you want to add an item to a vector, you call `conj`, which will return a *new* vector with the item added.

Why is this important? There are a few reasons: It means that the functions you write will usually be *pure* (have no side effects), which will make them easier to test and reason about. It makes sharing data throughout your program extremely safe, because distant parts of your application can't alter the states of each other accidentally.

For example, there's a bug in the following code: 

```javascript
var phoneNumbersPromise = getPhoneNumbers('george');

phoneNumbersPromise.then(function(phoneNumbers) {
    phoneNumbers.forEach(function(it) {
        it.type = normalizeNumberType(it);
    });
    
    renderAllPhoneNumbers(phoneNumbers);
});

phoneNumbersPromise.then(function(phoneNumbers) {
    var hasHomeNumber = phoneNumbers.some(function(n) {
        return n.type === 'HOME'
    });
    
    if(hasHomeNumber) renderHomePhoneNumber(phoneNumbers);
});

```

The behavior of one promise block depends on the behavior of the other. It doesn't matter what `normalizeNumberType` does, it affects the value of the phone number in a way that the second resolver will succeed or fail depending on execution order. Because we're dealing with asynchronous code, execution order may be non-deterministic. These two resolvers might be separated by tens of thousands of lines of code, or may have been added at different moments during development.

If the value returned by the phone number promise were immutable, this bug could not exist.

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

I believe there is value in learning new languages with unfamiliar ideas. Every programming language that's made it to production has been a successful attempt at solving a problem in software. The ideas encoded in these languages are often portable to other languages, or can be an introduction into new ways of thinking about a particular set of problems. I believe that the concepts embedded in Clojure are incredibly useful, and they will make you a better programmer in whatever language you choose to write in.

If you're looking to get started in Clojure, I highly recommend the book *Programming Clojure* by Stuart Halloway. From there, you can either try out some practical Clojure with the [Basic Om Tutorial][3], which covers building React-style UIs in ClojureScript, or you can try out the challenges on [CodeWars][4], which has a large-and-growing collection of Clojure challenges.


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
[3]: https://github.com/swannodette/om/wiki/Basic-Tutorial
[4]: https://www.codewars.com


