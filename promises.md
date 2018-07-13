- [HTML5 Rocks Promises](http://www.html5rocks.com/en/tutorials/es6/promises/)
#### Promises and `then`

When you promise your roommate that you're going to take out the trash, you
haven't done it yet, but it's something that you're going to do in the future
(usually). In JavaScript, promises work in a very similar way.  A `Promise`
object represents a value that may not be available yet, but will be resolved at
some point in the future. This allows us to write more flexible code. JavaScript
code typically is fired off line by line, one line immediately after the other.
By using `Promise` objects, we can escape this and have some code fire _only
when it ready_. The `fetch()` function returns a `Promise`. We can say, in code,
"ok, json, once you've got all the astronaut data, log it". Code immediately
after a `Promise` fires _before_ the `Promise` is resolved. This is the essence
of asynchronous JavaScript. 

`Promise` objects implement a `then` function that is called when the `Promise`
is *fulfilled*, or completed successfully. In the case of `fetch()`, a request is
sent to an API, 'http://api.open-notify.org/astros.json', and upon receiving a
response, the `Promise` is fulfilled (even if the response is an error).
Whatever is returned in the `Promise` is passed to the first `then` function.

Upon successful completion of our promise, whatever function we write inside of
`then()` will be called with the `Promise` response as the function's
_argument_.

One interesting thing about this `fetch()` code is that it highlights a powerful
feature of a `thenable` object — we can chain each `then` call, and the next one
receives the result of the previous one as its argument.

So, in this code:

```js
fetch('http://api.open-notify.org/astros.json')
  .then(response => response.json())
  .then(json => console.log(json));
```

...the line `then(response => response.json())` is getting the response
`response` from `fetch()` and using the `json` method to convert it into JSON.
That returned JSON is passed into the second `then`, `then(json =>
console.log(json))`, which logs the JSON data.

#### Body Mixin

The `fetch()` API includes a *mixin*, or additional code, called
[Body](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#Body),
which has functions that specialize in transforming the `body` of a request or
response, including `.json()`.

So calling `res.json()` in our `fetch()` is just a nice shorthand for saying "give
me the `body` of the response parsed as JSON".


