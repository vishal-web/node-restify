---
title: Response API
permalink: /docs/response-api/
---

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [Response][1]
    -   [cache][2]
    -   [noCache][3]
    -   [charSet][4]
    -   [header][5]
    -   [json][6]
    -   [link][7]
    -   [send][8]
    -   [sendRaw][9]
    -   [set][10]
    -   [status][11]
    -   [redirect][12]
    -   [redirect][13]
    -   [redirect][14]

## Response

**Extends http.ServerResponse**

Wraps all of the node
[http.ServerResponse][15]
APIs, events and properties, plus the following.

### cache

Sets the `cache-control` header.

**Parameters**

-   `type` **[String][16]** value of the header
                                       (`"public"` or `"private"`) (optional, default `"public"`)
-   `options` **[Object][17]?** an options object
    -   `options.maxAge` **[Number][18]** max-age in seconds

Returns **[String][16]** the value set to the header

### noCache

Turns off all cache related headers.

Returns **[Response][19]** self, the response object

### charSet

Appends the provided character set to the response's `Content-Type`.

**Parameters**

-   `type` **[String][16]** char-set value

**Examples**

```javascript
res.charSet('utf-8');
```

Returns **[Response][19]** self, the response object

### header

Sets headers on the response.

**Parameters**

-   `key` **[String][16]** the name of the header
-   `value` **[String][16]** the value of the header

**Examples**

If only key is specified, return the value of the header.
If both key and value are specified, set the response header.


```javascript
res.header('Content-Length');
// => undefined

res.header('Content-Length', 123);
// => 123

res.header('Content-Length');
// => 123

res.header('foo', new Date());
// => Fri, 03 Feb 2012 20:09:58 GMT
```

`header()` can also be used to automatically chain header values
when applicable:


```javascript
res.header('x-foo', 'a');
res.header('x-foo', 'b');
// => { 'x-foo': ['a', 'b'] }
```

Returns **[Object][17]** the retrieved value or the value that was set

### json

Syntatic sugar for:

```js
res.contentType = 'json';
res.send({hello: 'world'});
```

**Parameters**

-   `code` **[Number][18]?**    http status code
-   `body` **[Object][17]?**    value to json.stringify
-   `headers` **[Object][17]?** headers to set on the response

**Examples**

```javascript
res.header('content-type', 'json');
res.send({hello: 'world'});
```

Returns **[Object][17]** the response object

### link

Sets the link header.

**Parameters**

-   `key` **[String][16]**  the link key
-   `value` **[String][16]** the link value

Returns **[String][16]** the header value set to res

### send

Sends the response object. pass through to internal `__send` that uses a
formatter based on the `content-type` header.

**Parameters**

-   `code` **[Number][18]?** http status code
-   `body` **([Object][17] \| [Buffer][20] \| [Error][21])?** the content to send
-   `headers` **[Object][17]?** any add'l headers to set

**Examples**

You can use send() to wrap up all the usual writeHead(), write(), end()
calls on the HTTP API of node.
You can pass send either a `code` and `body`, or just a body. body can be
an `Object`, a `Buffer`, or an `Error`.
When you call `send()`, restify figures out how to format the response
based on the `content-type`.


```javascript
res.send({hello: 'world'});
res.send(201, {hello: 'world'});
res.send(new BadRequestError('meh'));
```

Returns **[Object][17]** the response object

### sendRaw

Like `res.send()`, but skips formatting. This can be useful when the
payload has already been preformatted.
Sends the response object. pass through to internal `__send` that skips
formatters entirely and sends the content as is.

**Parameters**

-   `code` **[Number][18]?** http status code
-   `body` **([Object][17] \| [Buffer][20] \| [Error][21])?** the content to send
-   `headers` **[Object][17]?** any add'l headers to set

Returns **[Object][17]** the response object

### set

Sets multiple header(s) on the response.
Uses `header()` underneath the hood, enabling multi-value headers.

**Parameters**

-   `name` **([String][16] \| [Object][17])** name of the header or
                                   `Object` of headers
-   `val` **[String][16]** value of the header

**Examples**

```javascript
res.header('x-foo', 'a');
res.set({
    'x-foo', 'b',
    'content-type': 'application/json'
});
// =>
// {
//    'x-foo': [ 'a', 'b' ],
//    'content-type': 'application/json'
// }
```

Returns **[Object][17]** self, the response object

### status

Sets the http status code on the response.

**Parameters**

-   `code` **[Number][18]** http status code

**Examples**

```javascript
res.status(201);
```

Returns **[Number][18]** the status code passed in

### redirect

Redirect is sugar method for redirecting.

**Parameters**

-   `options` **[Object][17]** url or an options object to configure a redirect
    -   `options.secure` **[Boolean][22]?** whether to redirect to http or https
    -   `options.hostname` **[String][16]?** redirect location's hostname
    -   `options.pathname` **[String][16]?** redirect location's pathname
    -   `options.port` **[String][16]?** redirect location's port number
    -   `options.query` **[String][16]?** redirect location's query string
                                        parameters
    -   `options.overrideQuery` **[Boolean][22]?** if true, `options.query`
                                                 stomps over any existing query
                                                 parameters on current URL.
                                                 by default, will merge the two.
    -   `options.permanent` **[Boolean][22]?** if true, sets 301. defaults to 302.
-   `next` **[Function][23]** mandatory, to complete the response and trigger
                           audit logger.

**Examples**

```javascript
res.redirect({...}, next);
```

A convenience method for 301/302 redirects. Using this method will tell
restify to stop execution of your handler chain.
You can also use an options object. `next` is required.


```javascript
res.redirect({
  hostname: 'www.foo.com',
  pathname: '/bar',
  port: 80,                 // defaults to 80
  secure: true,             // sets https
  permanent: true,
  query: {
    a: 1
  }
}, next);  // => redirects to 301 https://www.foo.com/bar?a=1
```

Returns **[undefined][24]** 

### redirect

Redirect with code and url.

**Parameters**

-   `code` **[Number][18]** http redirect status code
-   `url` **[String][16]** redirect url
-   `next` **[Function][23]** mandatory, to complete the response and trigger
                           audit logger.

**Examples**

```javascript
res.redirect(301, 'www.foo.com', next);
```

Returns **[undefined][24]** 

### redirect

Redirect with url.

**Parameters**

-   `url` **[String][16]** redirect url
-   `next` **[Function][23]** mandatory, to complete the response and trigger
                           audit logger.

**Examples**

```javascript
res.redirect('www.foo.com', next);
res.redirect('/foo', next);
```

Returns **[undefined][24]** 

[1]: #response

[2]: #cache

[3]: #nocache

[4]: #charset

[5]: #header

[6]: #json

[7]: #link

[8]: #send

[9]: #sendraw

[10]: #set

[11]: #status

[12]: #redirect

[13]: #redirect-1

[14]: #redirect-2

[15]: https://nodejs.org/docs/latest/api/http.html

[16]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[17]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[18]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[19]: #response

[20]: https://nodejs.org/api/buffer.html

[21]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error

[22]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean

[23]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function

[24]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined
