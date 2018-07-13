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

**Top-Tip:** Don't ever give out API keys or store them in a publicly
accessible place like a public or shared GitHub repository. In a production
setting, users' API keys would be stored securely in a database and not exposed
to other people. It is common for API keys to be stolen and used for nefarious
purposes, especially if the key is tied to any sort of banking or credit card
information.


## Explain What Are Sending `fetch()` Requests 

For our purposes, we will typically be sending `fetch()` requests to remote
servers. These servers contain APIs, programming interfaces that enable servers
receive requests for data they contain, find that data, and send it back in a
response. Many APIs are public or free, but require some sort of authentication.
We can use `fetch()` to access data on these, giving us access to all sorts of
information, from public transit delays and the current weather, to stock
information and satellite imagery.

#### Body Mixin

The `fetch()` API includes a *mixin*, or additional code, called
[Body](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#Body),
which has functions that specialize in transforming the `body` of a request or
response, including `.json()`.

So calling `res.json()` in our `fetch()` is just a nice shorthand for saying "give
me the `body` of the response parsed as JSON".
