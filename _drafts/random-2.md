# Immutable Datastructures for Fun and Profit

* What's an immutable data structure?
    * A data that doesn't change
    * We use immutable data structures all the time
        * Does anyone work in a language where String is mutable?
        * Does anyone *wish* String were mutable?
    * Imagine if `function` were a mutable object
    
* Why do you use them / what are they good for?
    * https://twitter.com/jcoglan/status/451467868409200640
    * A concrete example
* How do they work?
* Where do you get them from?



But aren't JS applications single-threaded? Who cares about concurrency?

JS apps run on a single thread, but they still support concurrency. That's what callbacks are!

````
// my-application.node.js
var currentUser = userService.getCurrentUser();
// currentUser = { name: 'David Nolen', username: 'dnolen', ... }

db.open('Contact', function(err, collection) {
    // currentUser = ????
});

````

````
var currentUser = userService.getCurrentUser();

db.open('Contact' ...);
// This code doesn't immediately execute, it yields itself to the
// JS run thread until the db library can open the collection

// << later >>
// Something calls:
userService.setCurrentUser({ name: 'Rich Hickey', ... });

// Which in turn executes:
merge(memoizedCurrentUser, newUser); // merges Rich with David

// << even later >>
(function(err, collection) {
    // currentUser = { name: 'Rich Hickey', ... }
    // Because the currentUser reference is the same as the memoizedCurrentUser
})

````

As long as you're:

1. sharing references to mutable objects
2. writing asynchronous code that uses those mutable objects

You're playing with fire

