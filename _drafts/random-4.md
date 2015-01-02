### Clojure for the Javascript Programmer

* I'm primarily a javascript programmer
* Some of you are also javascript programmers
* If you aren't, I won't hold it against you
* Java's a very... pretty... almost good language.

* About a year ago I started learning Clojure
* And have become a bit of a vocal enthusiast for it
  
  
---

### CLOJURE LOGO

* I'm especially interested in the ideas in clojure that can be ported to more familiar languages
  - I also think that the easiest way to understand those ideas is by becoming familiar with clojure
  - So I'm hoping that I can get you interested in dabbling in it a little bit

* What is Clojure?
  - Dynamically typed
  - Functional
  - Dialect of Lisp
  - For the JVM 
  - And as a compile-to-Javascript language

* This is intended to be a teaser for the language
  - Code samples can get really boring
  - And there are way more qualified people to speak to the details
  - I'll have some links at the end

* Before I go any further, though, a caveat. 
  - I don't know everything about clojure.
  - I don't know everything about functional programming.
  - I was a big FP skeptic until pretty recently.

* I'm going to start with an introduction to the syntax of clojure
  - And then go over concepts

---

### Not So Scary

* The most common thing I hear about clojure is that the syntax is intimidating.
* “Clojure seems really powerful, but when I look at that code I’m totally lost.”
* It's certainly pretty alien until you get used to it
* But at the heart of Clojure there are only two rules you need to know to orient yourself a little bit

* The first is that the opening parenthesis goes to the left.
  - This is unfamiliar, but not difficult.
  
* The second is that everything is a function
  - This is unfamiliar, but can definitely be tricky.
  - Here, "times" is a function that takes two argments, 4 and 2
  
* If we want to nest statements, we need to wrap them in parenthesis
  - This can look awkward, but has a couple advantages
  - It's never ambiguous
  - There's no distinctions between functions and expressions

* Here's what that nesting style would look like in javascript
  - I'm sure everyone has sen code that works like this

---

### Familiar Literals

* With the hard syntax stuff out of the way let's look at the easy syntax stuff
* Clojure has some a bunch of useful literals for describing the data in your program

* True, false. Nil is for "no value" or null.
* Numbers look the same
* Strings look the same -- but with double quotes only. Single quotes have a special meaning.
* This is called a Keyword.
  - It's a value that always means itself.
  - Like the Symbol type in Ruby.
  - It's like a Java Enum value, but without the enumerable part.
* This is vector.
  - It's almost exactly like an array. 
  - This one contains keywords.
* This is a map. 
  - Java-style maps - keys can be anything. 
  - This one maps keyword keys to string values.
* This is a set.
  - A set only stores unique items.
* And lastly, this is a list.
  - I just said that's a function call.
  - Programs are written in data structures.
  - In this case, this is a list with two items: a function name and the value 5.
  - At run-time lists are evaluated to produce results

* This sounds like academic foolishness
* But it lets you read your code just like you would read in any data.
* Any of the functions that work on the previous data structures, also works on your code.
* It's as easy to read as a JSON document.
* It makes it easy to modify programs programatically
  - write macros
  - perform static analysis
  
---

### AWESOME NAMES (BLANK)

* Before I leave the topic of syntax behind
* Clojure gives you a lot of freedom with the names you can give things
* Here are some valid function and value names

* By convention, a question mark at the end indicates a predicate
  - This function returns true if the argument provided is a number
* Functions that are unsafe are followed by a bang
  - This mutates a value to a new one
* Clojure functions use kebab-case
* Certain important or dynamic values in clojure have "earmuffs"
* Converters are often named what they convert. 
 - This one might converts clojurescript data structures into javascript ones
* And of course you can combine these rules to make pretty weird names
 - It's halloween soon, here's a sad witch

---

### Why Bother?

"Clojure feels like a general-purpose language beamed back from the near future." -- Programming Clojure

* It has a constellation of features that help you write better programs. 
* It's also pragmatic about letting you get a job done quickly -- no IO monads.
* It has features you didn't know you wanted, and you might find it hard to live without them.

---

### Write Less Code


* The first feature that you'll notice when learning clojure is how concise it can be.
* Programs in clojure can be half the size of the same program in other languages.
* Not *terse*
  - code golf is terse.
  - Minification is terse.
* Concise is understandable *and* compact.
  - JS is a much more expressive language than java.
  - It's less compact than Perl, perhaps, but Perl rapidly becomes unreadable.

* Here's a function in Javascript that checks if all the members of an array are numbers.
  - This should be very familiar, if a bit ugly.

* This is the exact same algorithm implemented in clojure. 
  - Not be familiar with the syntax, but I hope you can see the bones.
  - It's about 1/3 smaller.
* But it's also *simpler*.
  - We don't need to think about length of the collection
  - No index tracking
  - No off-by-one errors

---
## Write Less Code (contd.)

* But neither of these two functions are idiomatic to their language.
  - We can make both simpler to work with
  
* In Javascript we have the every function,
  - which abstracts the looping away from us.
* In clojure we also have an every-cue function,
  - clojure comes with a rich set of interrogative functions
  - we can use number-cue
  - comes in the standard library

---

### Think functionally

* In addition to writing shorter programs, you'll also be encouraged to think functionally.

* Functional programming is a hot topic 
  - Offers a few really interesting benefits
* Avoid acting on a mutable shared state
  - can be very difficult to reason about as a program gets larger
  - get to this more in a bit
* Easier to parallelize across many processors. 
  - Immutable values can be shared safely between processors
  - Function evaluation can be distributed across processors easily
* Tend to operate at a higher level of abstraction
  - Closer to describing the *nature* of your problem rather than the *details* of how to solve it.
* Like in the previous example, we could replace a loop with a construct that's simpler to think about

---

### 100 Functions on 1 Datastructure

* And speaking of simple, the core clojure API is pretty large and very comprehensive
  - But it's almost the only API you need for manipulating data.
* Data in clojure is entirely represented by the constructs I showed earlier - maps, lists, vectors, and sets.
  - So you have a large number of functions that can operate on these really rich data structures
  - Sounds a little like JS, right?
* Clojure libraries talk in these shared representations rather than exposing their own special universes of data
  - So... not like Java

---

### Embrace Immutability

* Does anyone here remember programming in a language that had mutable strings?
* Why do we have immutable strings and numbers -- but not immutable arrays and objects?
* It's kindof a funny question
  - We treat one class of data as "first movers" as atomic and indivisible
  - And another like a mutable ball of mud

* So why immutable data?
  - It makes it much easier to reason about your program
  - It's also the origin of a lot of difficult-to-reason about bugs
  - JS might not have parallelism, but it does have concurrency
    - Asynchronous programs are concurrent
    - And usually share data structures
    - Anyone ever had to deal with a race condition because some user events happened unexpectedly?
    - Or a couple ajax requests didn't arrive in the time you expected?
---

### Ellery Quote

* I was chatting with Ellery yesterday about React and he said this
  - React is just a javascript library, but it depends on an immutable outlook
  - Also, I didn't put him up to it
* If your data is immutable, a whole realm of badness just ... goes away.
* You don't even notice how bad you have it right now, until you experience a world that's better.

---

### Also...

* Live coding / REPL
* Hygenic macros
* ClojureScript
* Transducers
* Managed CSP
* Easy testability
* JVM / Javascript interop
* Value destructuring
* Ad-hoc Polymorphism
* Lazy and Infinite Seqs

---

### You Will Be A Better Programmer

* There is value in learning new languages with unfamiliar ideas. 
  - Every programming language in production has been a successful attempt at solving a problem
  - Ideas are encoded in languages
  - And are often portable to other languages -- like immutability
  - And can be an introduction into new ways of thinking
  - The concepts embedded in Clojure are incredibly useful, 
  - They will make you a better programmer in whatever language you choose to write in.

---

### Getting Started

---

### Coda

---

* I learned to sail last summer
* One of my side projects is to build a tool that displays a wind speed forecast over the next few days
* So I can bail on work for a few hours and go out in a boat for a bit and not end up paddling my way back
* This is a sample of the data I'm getting from a remote service

* And I want to display it like this
* A graph of wind speed per hour, divided into days

---

* This is the implementation in clojure
* It takes the hourly data and breaks it up into local daily chunks
* I drop the last because I don't want partial data
* And I drop the first because I have a better way to get wind data for today

---

* As an exercise, I did the same thing in underscore.
* Incidentally, this is the first underscore code I've ever needed to write

---

* Here's a bug in Javascript
* We have a promise that gets a user's phone numbers, probably from the server, but it doesn't really matter.
* And we have one resolver that renders all the phone numbers
* And another resolver that determines if we have a home address
* Does anyone see the bug?

