# AJAX Miscellany

Sometimes topics just don't naturally fit. In this lesson we're going to
provide a few valuable bits of reference that will help you build on top of
basic `fetch()` usage.

## Objectives

1. Describe authenticated `fetch()`
2. Demonstrate Converting Response Bodies

#### Authenticated Fetching

So far, we've been making our requests to a public API, e.g.
'http://api.open-notify.org/astros.json', that responds openly to anything that
requests information.

Since APIs are hosted on servers and do cost money to maintain, many APIs
require a bit more proof that users are real and not bots out to abuse their
servers.

Most often, APIs that require authentication provide a way to generate an API
key for you to use.  This API key is then included in the `fetch()` request as
part of a second argument, an `Object` whose properties have additional data:

```js
fetch('url-example.com', {
  headers: {
    Authorization: `<your API key here>`
  }
})
```

Most APIs we send `fetch()` requests to will usually have their own rules and
documentation on how to structure requests and what is required to gain access
to their API data.

> **Top-Tip:** Don't ever give out API keys or store them in a publicly
> accessible place like a public or shared GitHub repository. In a production
> setting, users' API keys would be stored securely in a database and not
> exposed to other people. It is common for API keys to be stolen and used for
> nefarious purposes, especially if the key is tied to any sort of banking or
> credit card information.

## Demonstrate Converting Response Bodies

The `fetch()` method returns a `Promise` whose `then()` code will be passed
status of the network request as an argument:

```
fetch("URL")
  .then(resp => ...)

This `resp` object has a number of methods in it which can take the page's body
and turn it into a variety of forms: plain text, JSON, a "blob", etc. These
options are listed [here][Body].

So, calling `res.json()` in our `fetch()` is just a nice shorthand for saying
"give me the `body` of the response parsed as JSON".

[Body]: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#Body

## Conclusion

Two edge topics are authentication and options for converting the data our
queried APIs return.
