Application structure in the DOM

We started building applications on top of a document.

Attach a thing here a thing there.

Store some state over there.

And eventually we realize how much trouble we're in.

So we refactor. We use an MVC library.

And for a while, this is good. You store the state in one place, behind a shield, and only update it by talking through the keyhole.

But what about the things that's only half the problem.

There's state 


Web Application Development: JS in the Large
===========================

**Abstract:** A philosophical talk on the structure of JS applications; How we can maintain javascript applications when moving from programming in the small to programming in the large? What can we learn from other languages and approaches?

----

This is going to be a philosophical talk. There's a little code at the end, but it's not tied to a particular implementation. I'm not going to talk about implementations in particular, but rather to get you thinking about writing big applications with Javascript.

What do I mean by programming in the large? The term comes from a paper in 1975 by Frank DeRemer and Hans Kron. Programming in the small is solving a small, unchanging problem with an algorithm, programming in the large is solving a large, mutating problem with a system. 

It describes systems that can have many possible states, or infinite states. It describes systems that can take many possible actions at any moment, or take many actions at once within different modules. It describes a system that has been around, touched by many hands, and in a constant state of reconstruction.

Sounds a little bit like a complicated frontend app, right?

----

Let's talk about history. In the beginning, there was the document.

----

The server built the markup -- if it wasn't just static HTML sitting on disk, and the server kept the state of the app somewhere… out there.

It does some simple get requests with the server, some posts to update the server state with new data.

Those were simple days, weren't they? The user expperience was terrible, but the whole internet's user experience was terrible. If your site came back in less than a second, woah boy, you're hot shit. Remember when Google would tell you how long it took to build its results? A billion results in 35 milliseconds.

----

So the ajax-and-web-two-point-oh thing comes along. While we don't know it yet, we are no longer serving documents to our users, we're serving applications.

We start to wire event listeners into our application, and we start talking to the server asynchronously.

At some point, we decide that we need to store a little bit of that server's state in the client. It means we don't need to go back to the server for state info, and it's difficult to store that state in the session anyway. 


----

TTD: Channels and queue example.
