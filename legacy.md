# Legacy AJAX with XMLHttpRequest

## Problem Statement

If you're learning JavaScript in 2018, you should reach for `fetch()` when
writing AJAX code. But when looking at old code bases, you might find
object-oriented and event-based tool called `XMLHttpRequest`.

We'll provide a brief description  of the code.

## Objectives

1. Explain creating an `XMLHttpRequest` instance
2. Demonstrate adding success event handler
3. Demonstrate adding error event handler
4. Demonstrate executing the request

## Explain Creating An `XMLHttpRequest` Instance

The original approach involved creating new _instances_ of the `XMLHttpRequest`
_class_. You might not be deeply familiar with object-oriented programming, but
the following code means: "Create a new thing that can make network requests."

```js
let xhr = new XMLHttpRequest();
```

It might be surprising that the class that makes these JSON requests is called
an `XMLHttpRequest`. When `XMLHttpRequest` was creatd, XML was a very popular
serialization format. JSON has, since, become more popular.

Great, so now we have an instances of `XMLHttpRequest`. We need to configure it
to make a request.

```js
xhr.open('GET', 'String of URL');
xhr.responseType = 'json';
```

The second argument to `open()` is the data source. The first argument, `GET`
will usually be left alone. We now have an instance that we will invoke
`send()` on. As the instance does things, it will trigger events. If you have
event listeners set on the instance, they will be able to use the data the
instance got.

## Demonstrate Adding Success Event Handler

One event that fires when an `XMLHttpRequest` instance successfully retrieves
data is a `load` event. To set a listener for it do the following:

```js
xhr.addEventListener("load", data => console.log("Wow I got: " + xhr.response))
```

## Demonstrate Adding Error Event Handler

One event that fires when an `XMLHttpRequest` instance successfully retrieves
data is an `error` event. To set a listener for it do the following:

```js
xhr.addEventListener("error", data => console.error("OH NO!"))
```
## Demonstrate Executing The Request

After your instance is configured and your listeners are set, you can send the
request: `xhr.send()`.

### Putting It All Together

```js
//set up the request
let xhr = new XMLHttpRequest();
xhr.open('GET', 'http://api.open-notify.org/astros.json');
xhr.responseType = 'json';

//provide a function to call if the request is successful
xhr.addEventListener("load", data => console.log("Wow I got: " + xhr.response))

//provide a function to call if the request is not successful
xhr.addEventListener("error", data => console.error("OH NO!"))

//send the request
xhr.send();
```

## Conclusion

Getting remote data in JavaScript has traditionally required a fair amount of
plumbing to make things happen. In the past, the only option was something
called an `XMLHttpRequest`, also referred to as XHR. It's not that it's *hard*
to get data using XHR, but it does take quite a bit of setup.

With a knowledge of binding events and creating new instances from classes in
JavaScript, you should be able to examine and read any legacy `XMLHttpRequest`
code you see.

