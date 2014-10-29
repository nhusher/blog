---

title: Type Blindness

kerned_title: Type Blindness

layout: post

---

The presentation on Clojure went well and the feedback has been very positive. During the Q&A part of the talk, I fielded a question that's been bouncing around in my head a lot, which can be summarized: "How do you handle the fact that Clojure is a dynamically-typed language? I rely on my environment to catch the errors I make regarding type."

I punted on the question, because I'm a JS programmer who is pretty comfortable with dynamic types, and have been living without a compile-time type checker for most of my professional career. I've written a fair share of Java (mostly unhappily) and a little TypeScript (mostly-contendedly), so I'm familiar with the benefits.

My general position is that I'm skeptical of these benefits; they're either of limited value, or only valuable if you concede that typed data is necessary. Once you've made the concession that data inherently has *types* -- rather than expressing *behaviors* -- a complex world of problems suddenly manifests, without necessarily solving any problems. I haven't measured this, but I belive that the kinds of errors I tend to make while writing aren't the kind that would be picked up by even the most rigorous of type checkers.


* One advantage of a type checker would be that it would require me to think more rigorously about my program.
* Data modeling when you know the least about a system
* Type annotations are a programmer game to feel better
* Type annotations subduct many meanings of "type"




But I am now concerned that being comfortable with this level of ambiguity has made me over-skeptical of the value of types, and particularly type annotations.

This is made more complex by a recent piece by Stephen Kell, *Seven deadly sins of talking about “types”*. The first "deadly sin" speaks to this most clearly:

> **Not distinguishing abstraction from checking**
>
> This is my number-one beef. Almost all languages offer data abstraction. Some languages proceed to build a static checking system on top of this. But please, please, don't confuse the two.
>
> We see this equivocation used time and time again to make an entirely specious justification of one language or other. We see it used to talk up the benefits of type systems by hijacking the benefits of data abstraction, as if they were the same thing.

Both Clojure and Javascript have data abstraction mechanisms. Clojure has protocols, records, and structs. Javascript has constructed objects and prototypal inheritance.[1]


And later:

> Instead of saying “type” or “type system”, I encourage anyone to check whether anything from the following alternative vocabulary will communicate the intended meaning. Deciding which term is appropriate is a good mental exercise for figuring out what meaning is actually intended.
> 
> * data abstraction
> * data type
> * predicate, proposition
> * proof, proof system
> * interface specification [language]
> * [compile-time] checker
> * specification, verification




* 1: It's pretty simple to build a basic type system on top of Javascript with a little understanding of the language constructs. It's not usually a good idea, but it's possible.

[In Search Of Types]: http://www.cl.cam.ac.uk/~srk31/research/papers/kell14in-author-version.pdf