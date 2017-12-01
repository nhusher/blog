---

title: Using context in redux-saga

layout: post

canonical: http://blog.faraday.io/context-in-redux-saga/

---

*This post originally appeared on the [Faraday Blog]*

At [work], we use [redux-saga] in our client-side webapp because it offers a robust and testable set of tools for managing complex and asynchronous effects within Redux apps. I recently upgraded our version of redux-saga from an embarrassingly old version to the latest, in part to take advantage of a new (but largely undocumented) feature: context.

### The problem

I want to be able to write sagas without having to implicitly import any statically-declared singletons. The reason for that is simple: if I'm testing the saga that creates new users, I don't want to drag along the API layer or the router instance.

There are also a few modules in the codebase that *only* work in a browser and will fail loudly when I try to run them in mocha.  If one of these browser-only modules ends up in an import chain, suddenly my tests start failing for bad reasons. At worst, I have to rethink my testing strategy to include a mock browser environment. I'm lazy and hate testing, and I think mocks are a gross awful code smell that should be avoided at all costs.

### The solution

One of redux-saga's greatest features is that I can test a saga's behavior without actually executing that behavior, or even really knowing much about the details of how that behavior alters the world. It it should be possible to apply that to dependencies as well. And it is possible in redux-saga 0.15.0 and later using context.

Right now, there are very limited docs for context, but acts as shared value across all sagas that can be read with the `getContext` effect and written to by the `setContext` effect. It can also be set when the saga middleware is created by passing a context object as configuration.

### Example

Let's say we're writing an app that fetches game inventory data from the server using some kind of authentication. We don't want tokens and authentication to bleed into all our sagas, so we wrap it up in a nice singleton API service:

```js
class ApiService {
  getInventory = () => {
    return fetch('/api/inventory', {
      headers: {
        Authorization: `Bearer ${this.token}`
      }
    }).then(res => res.json())
  }
}

const api = new ApiService()
api.token = localStorage.token

export default api
```

Without context, our saga would probably statically import the API singleton:

```js
import { call } from 'redux-saga/effects'
import api from './api' 

export function * fetchInventorySaga () {
  const inventory = yield call(api.getInventory)
  // Do something with the inventory data...
}
```

All is well until we try to test it in nodejs/mocha:

```js
import { fetchInventorySaga } from '../src/sagas/inventorySaga.js
// ReferenceError: localStorage is not defined
```

There is no localStorage in the nodejs global context. We can either pull in a testing harness to change how `import api from './api'` is resolved, attempt to run the tests in a browser, or roll our own late-binding mechanism so that you don't need to import API and can pass the API instance in at runtime.

We need to solve the same problem for `fetch`, because that's also absent in nodejs.

Or, we could use context. Our saga changes only a little bit:

```js
import { call, getContext } from 'redux-saga/effects'
// No more API import:
// import api from './api' 

export function * fetchInventorySaga () {
  // Get the api value out of the shared context:
  const api = yield getContext('api')

  const inventory = yield call(api.getInventory)
  // Do something with the inventory data...
}
```

We can now test the `getContext` effect just like we would any other redux-saga effect, and we can insert a mock value into `fetchInventorySaga` at test-time if we need to.

Setting up context in the main application is very straightforward. When you're creating your saga middleware:

```js
import createSagaMiddleware from 'redux-saga'
import api from './api'

const saga = createSagaMiddleware({
  context: {
    api // our singleton API value
  }
})
```

Being able to late-bind singleton values like this has been enormously helpful writing robust tests in a complex codebase. I'll be steadily migrating the application code to use `getContext` more frequently, now that I have it as an option.

[Faraday Blog]: http://blog.faraday.io/context-in-redux-saga/
[work]: https://www.faraday.io
[redux-saga]: https://redux-saga.js.org
[a couple cryptic sentences]: https://github.com/redux-saga/redux-saga/releases/tag/v0.15.0