# AJAX - Getting started

AJAX or Asynchronous Javascript and XML is a old way of sending and receiving data in various formats (XML, JSON, text, ...). AJAX can make requests to the server without reloading the page, which makes it great for web development.

Two main things to talk about when we are discussing about AJAX are the `fetch` function and a thing called `Promise`. The `fetch` command is a way of getting data from servers, and a `Promise` is a value that will be returned at some point in the execution, but not right away. When dealing with promises we could give to them a `.then` method, and this `.then` will describe what should be done when we get a response back from the servers.

**Important information:** The argument of `.then` is always a function (it can also be a anonymous function with parameters, but always a function).

# The fetch() function

The `fetch()` method is a modern and versatile way of dealing with asynchronous Javascript supported only by modern browsers, this syntax is expressed like:

```jsx
let promise = fetch(url, [options]);
```

`url` represents the url to access in order to exchange information, and the `[options]` argument represents all the options that we could have for this operation. Remember that when no option is provided the fetch will be a GET request.

The fetch function will works as follow:

1. The browser start the request and return a promise.
2. The `Promise` returned, resolves in a `Response` object as soon as the server responds.
    - We can get this response status by checking if the value of the variable `Response.status` is `200` or if `Response.ok` returned true.
3. To get the response body, we need to use an additional method call.
    - `response.text()`
    - `response.json()`
    - `reponse.formData()`
    - `response.blob()`
    - `response.arrayBuffer()`

Important information: We can choose only one body-reading method.

When dealing with `fetch()` is also very common the use of `.catch` to catch any errors and appropriately respond to this eventual errors. (also remember that `.catch` also takes a function as argument).

Alongside with .catch, another common practice is to prevent the browser to refresh after a form submission, to do that we only need to add `return false;` at the end of a `onsubmit()` method.

## Response headers

Response headers are "properties" of a response, that doesn't relate with the content of the message itself. For example we could have this response headers after a get request.

```jsx
200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
...
```

You could get individual headers or iterate over them using `response.header`

## Request headers

Request headers are the same as response headers, although they are in "request side", that means, they are in send to a server. For example:

```jsx
GET /home.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
...
```

## Post requests and any other requests

To make a `POST` or any other request, we need to use fetch options:

- `method` - HTTP-method, for example: `POST`, `PUT`, ...
- `body` - the request body, that could be:
    - a string (commonly a JSON-encoded)
    - `FormData` object, to submit the data as `form/something`,
    - `Blob`, to send binary data

```jsx
/* Code snippet to submit a user object as JSON */
let user = {name: 'Jhon', surname:'Smith'};

let response = await fetch('some/crazy/url', {
	method: 'POST',
	headers: { // Headers most of time are optional
		'Content-Type': 'application/json; charset=utf-8'
	},
	body: JSON.stringify(user)
});

let result = await response.json();
alert(result.message);
```