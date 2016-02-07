---

title: Transaction cancellation with redux-saga

layout: post

---

I've been working on translating the [Faraday app] to use redux as its primary state-management tool for a few days. I've been using redux-saga as a way to manage complex asynchronous code, and it's worked out nicely thus far. I want to cover one aspect in particular, because it was a tricky problem to solve.

The JSON API that the Faraday app uses is fairly stateful, and it's tough to write obvious javascript logic for.

The best example of this is *audiences*. This is the prime mover in the Faraday UI universe. Audiences are associated with a UUID and are (mostly) immutable. Generating a new audience, though, is fairly complex: First some geography data must be requested, then the new audience information is pushed to the backend. The backend immediately responds with the UUID of the audience, and the frontend then immediately requests the audience information at that UUID. This process can take as long as 45 seconds for large and complex audiences when the system is under load. The user can also manipulate the UI in such a way that we want to bail out of whatever work we were doing.

Redux-saga has made this problem much easier to solve. It is a plugin for redux that creates a new asynchrony-management primitive, the saga, which describes a series of actions that can be taken that can produce side effects into the redux store. On the surface, it offers similar features to redux-thunk or redux-promise, though I think it has a few unique advantages.

The first advantage is that it's incredibly easy to test without doing any mocking. A saga is an ES6 generator function, and the harness that wraps the generator exposes an API that makes it possible to declare what a saga will do without producing any side effects. When run standalone, you can test against the intended side effects without mocking the whole system, but it will operate as expected when plugged into the Redux middleware.



[Faraday app]: https://app.faraday.io