1. Explain how JavaScript fetches data from remote resources
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

