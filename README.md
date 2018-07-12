Fetching Asynchronously with JavaScript
---

## The Problem with Data

Users of the modern web expect a high level of responsiveness and a
_seamless_ experience when visiting a website.




There is research that shows that
40 percent of visitors to a website will leave if the site takes more than 3
seconds to load. This number becomes higher when mobile visitors are taken into
account.

As web developers, making sure the websites we build are fast and responsive to
users is critical. But when it comes to loading content, we often find ourselves
needing a lot of data.

Data takes memory to store, and bandwidth to download from a server. But just as
importantly to the end user, loading a lot of data at once can also take a
noticeable amount time, especially on a slow connection.

JavaScript provides a solution by allowing us to serve up data content
_separately_ from the initial page load of a site. The result is that a user
visiting a website will see _something_ load quickly, the initial DOM. When the
rest of the data is ready, JavaScript can be used to add it to the DOM.

This opens up a lot of possibilities!

* It saves on bandwidth. We're not loading the entire page over any over, only
what we need
* It allows us to pull in dynamic content. The same HTML page could be used for
every recipe on a cooking website, only the text content changes.
* It allows us to get data from multiple sources. We could make a website that
displays the current weather forecast and the current price of bitcoin side by
side!

Overall, allowing JavaScript to request, receive, and render data results in a
much better user experience. In this lesson, we will be discussing how
JavaScript retrieves data along with how to use the built-in `fetch()` function to
handle remote data retrieval.

## Objectives

1. Explain how JavaScript fetches data from remote resources
2. Explain how `fetch()` is used in modern browsers
3. Explain what are sending `fetch()` requests 
4. Working around backwards compatibility issues

## Explain How JavaScript Fetches Data From Remote Resources

Getting remote data in JavaScript has traditionally required a fair amount of
plumbing to make things happen. In the past, the only option was something
called an `XMLHttpRequest`, also referred to as XHR. It's not that it's *hard*
to get data using XHR, but it does take quite a bit of setup.

Let's say we wanted to retrieve a list of all the people currently in space. The
list is freely available as an API here:
[http://api.open-notify.org/astros.json][http://api.open-notify.org/astros.json]

Using the old ways of XHR, getting that data would look something like this:

```js
//set up the request
let xhr = new XMLHttpRequest();
xhr.open('GET', 'http://api.open-notify.org/astros.json');
xhr.responseType = 'json';

//provide a function to call if the request is successful
xhr.onload = function() {
 console.log(xhr.response);
};

//send the request
xhr.send();
```

The above code works. The `xhr.response` will include the names of all the
astronauts in space (sent back from the API we requested from), but that's a lot
of setup to just say "give me some JSON from this URL". There are alternatives,
such as jQuery's `$.ajax()`, that applied some syntactic sugar on top of
`XMLHttpRequest`s, but ultimately, there should be an even simpler way to send
requests for remote data.

## Explain How `fetch()` is Used in Modern Browsers

The `fetch()` function is a new API for retrieving resources. It's a global
function, which means no creating new XHR objects, and it streamlines resource
requests. Let's try that call to the Astros API again.

```js
fetch('http://api.open-notify.org/astros.json')
  .then(response => response.json())
  .then(json => console.log(json));
```

![kimmy wow](http://i.giphy.com/3osxYwZm9WZwnt1Zja.gif)

It looks a lot better, right? Let's break it down.

Since we're making a simple `GET` request (asking for data, not `POST`ing, or
sending data), we can just pass the URL directly to `fetch()`. But what's
happening next?

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

## Explain What Are Sending `fetch()` Requests 

For our purposes, we will typically be sending `fetch()` requests to remote
servers. These servers contain APIs, programming interfaces that enable servers
receive requests for data they contain, find that data, and send it back in a
response. Many APIs are public or free, but require some sort of authentication.
We can use `fetch()` to access data on these, giving us access to all sorts of
information, from public transit delays and the current weather, to stock
information and satellite imagery.

#### Authenticated Fetching

So far, we've been making our requests to a public API,
'http://api.open-notify.org/astros.json', that responds openly to anything that
pings it. Since APIs are hosted on servers and do cost money to maintain, many
APIs require a bit more proof that users are real and not bots out to abuse
their servers.

Most often, APIs that require authentication provide a way to generate an API
key for you to use.  This API key is then included in the `fetch()` request as
part of a second argument.

```js
fetch('url-example.com', {
  headers: {
    Authorization: `<your API key here>`
  }
})
```

Each API we send `fetch()` requests to will usually have their own rules and
documentation on how to structure requests and what is required to gain access
to their API data.

**Top-Tip:** Don't ever give out API keys or store them in a publicly accessible place like a
public or shared GitHub repository. In a production setting, users' API keys would be
stored securely in a database and not exposed to other people. It is common for
API keys to be stolen and used for nefarious purposes, especially if the key is
tied to any sort of banking or credit card information.

## Working Around Backwards Compatibility Issues

As you can see, `fetch()` provides us with a clean, low-maintenance way to
get and work with resources.

Keep in mind that, while it is increasing, [browser
support](http://caniuse.com/#feat=fetch) for `fetch()` is still limited to roughly
90% of users. For the remaining 10%, we still need to consider the use of XHR or
jQuery's `$.ajax` as fallback options.

## Conclusion

Modern websites now often have just _one_ HTML page, which loads initially. As
you navigate around one of these sites, instead of jumping from HTML page to
HTML page, JavaScript is loading the exact content we need and swapping it in
when its ready. Using `fetch()`, we can easily include requests for data wherever
we need to in our code. We can `fetch()` data on the click of a button or the
expansion of a tab, instead of when the page initially loads.

By doing this, we can send _only_ the data that is needed, saving on server
bandwidth while minimizing loading times for the end user. Learning how to use
`fetch()` also opens up the world of data for us to access and incorporate in our
own projects. 

## Resources

- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [HTML5 Rocks Promises](http://www.html5rocks.com/en/tutorials/es6/promises/)

<p class='util--hide'>View <a href='https://learn.co/lessons/javascript-fetch'>Getting Data from the Web</a> on Learn.co and start learning to code for free.</p>

[space]: https://www.warnerbros.com/archive/spacejam/movie/jam.htm
