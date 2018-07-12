Fetching Asynchronously with JavaScript
---

## Problem Statement

When it comes to making engaging web sites, we often find ourselves needing to
send a lot of data (text, images, media, etc.). But sending and loading this
data on page visit *feels* slow, especially on a slow connection.

However, web users expect sites to load quickly and to stay updated. Research
shows that 40 percent of visitors to a website will leave if the site takes
more than 3 seconds to load. Mobile users are even less patient.

We deliver sites that don't bore users by using a technique called ***AJAX***.
In AJAX we:

1. Deliver an initial, engaging page using HTML and CSS
2. Use JavaScript to add more to the DOM, behind the scenes

In this lesson, we will use the JavaScript `fetch()` function and experience
the AJAX technique.

## Objectives

2. Explain how `fetch()` is used in modern browsers
3. Explain what are sending `fetch()` requests 
4. Working around backwards compatibility issues
5. Identify examples of the AJAX technique on popular web sites


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

## Identify Examples Of The AJAX Technique On Popular Web Sites

The AJAX technique opens up a lot of possibilities!

* It allows us to pull in dynamic content. The same "framing" HTML page could
  be used for every recipe on a cooking website, only the text content changes.
  This approach was pioneered by GMail whose navigational area is swapped for
  mail content swiftly thanks to AJAX.
* It allows us to get data from multiple sources. We could make a website that
  displays the current weather forecast and the current price of bitcoin side
  by side! This approach is used by most sites to render ads. Your content
  loads while JavaScript gets the ad to show and injects it into your page.

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
