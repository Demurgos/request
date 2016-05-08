# request(options, callback)

The first argument can be either a `url` or an `options` object. The only required option is `uri`; all others are optional.

- `uri` || `url` - fully qualified uri or a parsed url object from `url.parse()`
- `baseUrl` - fully qualified uri string used as the base url. Most useful with `request.defaults`, for example when you want to do many requests to the same domain.  If `baseUrl` is `https://example.com/api/`, then requesting `/end/point?test=true` will fetch `https://example.com/api/end/point?test=true`. When `baseUrl` is given, `uri` must also be a string.
- `method` - http method (default: `"GET"`)
- `headers` - http headers (default: `{}`)

---

- `qs` - object containing querystring values to be appended to the `uri`
- `qsParseOptions` - object containing options to pass to the [qs.parse](https://github.com/hapijs/qs#parsing-objects) method. Alternatively pass options to the [querystring.parse](https://nodejs.org/docs/v0.12.0/api/querystring.html#querystring_querystring_parse_str_sep_eq_options) method using this format `{sep:';', eq:':', options:{}}`
- `qsStringifyOptions` - object containing options to pass to the [qs.stringify](https://github.com/hapijs/qs#stringifying) method. Alternatively pass options to the  [querystring.stringify](https://nodejs.org/docs/v0.12.0/api/querystring.html#querystring_querystring_stringify_obj_sep_eq_options) method using this format `{sep:';', eq:':', options:{}}`. For example, to change the way arrays are converted to query strings using the `qs` module pass the `arrayFormat` option with one of `indices|brackets|repeat`
- `useQuerystring` - If true, use `querystring` to stringify and parse
  querystrings, otherwise use `qs` (default: `false`).  Set this option to
  `true` if you need arrays to be serialized as `foo=bar&foo=baz` instead of the
  default `foo[0]=bar&foo[1]=baz`.

---

- `body` - entity body for PATCH, POST and PUT requests. Must be a `Buffer`, `String` or `ReadStream`. If `json` is `true`, then `body` must be a JSON-serializable object.
- `form` - when passed an object or a querystring, this sets `body` to a querystring representation of value, and adds `Content-type: application/x-www-form-urlencoded` header. When passed no options, a `FormData` instance is returned (and is piped to request). See "Forms" section above.
- `formData` - Data to pass for a `multipart/form-data` request. See
  [Forms](guides/forms.md) section above.
- `multipart` - array of objects which contain their own headers and `body`
  attributes. Sends a `multipart/related` request. See [Forms](guides/forms.md) section
  above.
  - Alternatively you can pass in an object `{chunked: false, data: []}` where
    `chunked` is used to specify whether the request is sent in
    [chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding)
    In non-chunked requests, data items with body streams are not allowed.
- `preambleCRLF` - append a newline/CRLF before the boundary of your `multipart/form-data` request.
- `postambleCRLF` - append a newline/CRLF at the end of the boundary of your `multipart/form-data` request.
- `json` - sets `body` to JSON representation of value and adds `Content-type: application/json` header.  Additionally, parses the response body as JSON.
- `jsonReviver` - a [reviver function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) that will be passed to `JSON.parse()` when parsing a JSON response body.
- `jsonReplacer` - a [replacer function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) that will be passed to `JSON.stringify()` when stringifying a JSON request body.

---

- `auth` - A hash containing values `user` || `username`, `pass` || `password`, and `sendImmediately` (optional).  See documentation above.
- `oauth` - Options for OAuth HMAC-SHA1 signing. See documentation above.
- `hawk` - Options for [Hawk signing](https://github.com/hueniverse/hawk). The `credentials` key must contain the necessary signing info, [see hawk docs for details](https://github.com/hueniverse/hawk#usage-example).
- `aws` - `object` containing AWS signing information. Should have the properties `key`, `secret`. Also requires the property `bucket`, unless you’re specifying your `bucket` as part of the path, or the request doesn’t use a bucket (i.e. GET Services). If you want to use AWS sign version 4 use the parameter `sign_version` with value `4` otherwise the default is version 2. **Note:** you need to `npm install aws4` first.
- `httpSignature` - Options for the [HTTP Signature Scheme](https://github.com/joyent/node-http-signature/blob/master/http_signing.md) using [Joyent's library](https://github.com/joyent/node-http-signature). The `keyId` and `key` properties must be specified. See the docs for other options.

---

- `followRedirect` - follow HTTP 3xx responses as redirects (default: `true`). This property can also be implemented as function which gets `response` object as a single argument and should return `true` if redirects should continue or `false` otherwise.
- `followAllRedirects` - follow non-GET HTTP 3xx responses as redirects (default: `false`)
- `maxRedirects` - the maximum number of redirects to follow (default: `10`)
- `removeRefererHeader` - removes the referer header when a redirect happens (default: `false`). **Note:** if true, referer header set in the initial request is preserved during redirect chain.

---

- `encoding` - Encoding to be used on `setEncoding` of response data. If `null`, the `body` is returned as a `Buffer`. Anything else **(including the default value of `undefined`)** will be passed as the [encoding](http://nodejs.org/api/buffer.html#buffer_buffer) parameter to `toString()` (meaning this is effectively `utf8` by default). (**Note:** if you expect binary data, you should set `encoding: null`.)
- `gzip` - If `true`, add an `Accept-Encoding` header to request compressed content encodings from the server (if not already present) and decode supported content encodings in the response.  **Note:** Automatic decoding of the response content is performed on the body data returned through `request` (both through the `request` stream and passed to the callback function) but is not performed on the `response` stream (available from the `response` event) which is the unmodified `http.IncomingMessage` object which may contain compressed data. See example below.
- `jar` - If `true`, remember cookies for future use (or define your custom cookie jar; see examples section)

---

- `agent` - `http(s).Agent` instance to use
- `agentClass` - alternatively specify your agent's class name
- `agentOptions` - and pass its options. **Note:** for HTTPS see [tls API doc for TLS/SSL options](http://nodejs.org/api/tls.html#tls_tls_connect_options_callback) and the [TLS/SSL guide](guides/tls-ssl-protocol.md#using-optionsagentoptions).
- `forever` - set to `true` to use the [forever-agent](https://github.com/request/forever-agent) **Note:** Defaults to `http(s).Agent({keepAlive:true})` in node 0.12+
- `pool` - An object describing which agents to use for the request. If this option is omitted the request will use the global agent (as long as your options allow for it). Otherwise, request will search the pool for your custom agent. If no custom agent is found, a new agent will be created and added to the pool. **Note:** `pool` is used only when the `agent` option is not specified.
  - A `maxSockets` property can also be provided on the `pool` object to set the max number of sockets for all agents created (ex: `pool: {maxSockets: Infinity}`).
  - Note that if you are sending multiple requests in a loop and creating
    multiple new `pool` objects, `maxSockets` will not work as intended.  To
    work around this, either use [`request.defaults`](#requestdefaultsoptions)
    with your pool options or create the pool object with the `maxSockets`
    property outside of the loop.
- `timeout` - Integer containing the number of milliseconds to wait for a
server to send response headers (and start the response body) before aborting
the request. Note that if the underlying TCP connection cannot be established,
the OS-wide TCP connection timeout will overrule the `timeout` option ([the
default in Linux can be anywhere from 20-120 seconds][linux-timeout]).

[linux-timeout]: http://www.sekuda.com/overriding_the_default_linux_kernel_20_second_tcp_socket_connect_timeout

---

- `localAddress` - Local interface to bind for network connections.
- `proxy` - An HTTP proxy to be used. Supports proxy Auth with Basic Auth, identical to support for the `url` parameter (by embedding the auth info in the `uri`)
- `strictSSL` - If `true`, requires SSL certificates be valid. **Note:** to use your own certificate authority, you need to specify an agent that was created with that CA as an option.
- `tunnel` - controls the behavior of
  [HTTP `CONNECT` tunneling](https://en.wikipedia.org/wiki/HTTP_tunnel#HTTP_CONNECT_tunneling)
  as follows:
   - `undefined` (default) - `true` if the destination is `https`, `false` otherwise
   - `true` - always tunnel to the destination by making a `CONNECT` request to
     the proxy
   - `false` - request the destination as a `GET` request.
- `proxyHeaderWhiteList` - A whitelist of headers to send to a
  tunneling proxy.
- `proxyHeaderExclusiveList` - A whitelist of headers to send
  exclusively to a tunneling proxy and not to destination.

---

- `time` - If `true`, the request-response cycle (including all redirects) is timed at millisecond resolution, and the result provided on the response's `elapsedTime` property.
- `har` - A [HAR 1.2 Request Object](http://www.softwareishard.com/blog/har-12-spec/#request), will be processed from HAR format into options overwriting matching values *(see the [HAR 1.2 section](guides/support-for-har-1.2.md) for details)*
- `callback` - alternatively pass the request's callback in the options object

The callback argument gets 3 arguments:

1. An `error` when applicable (usually from [`http.ClientRequest`](http://nodejs.org/api/http.html#http_class_http_clientrequest) object)
2. An [`http.IncomingMessage`](http://nodejs.org/api/http.html#http_http_incomingmessage) object
3. The third is the `response` body (`String` or `Buffer`, or JSON object if the `json` option is supplied)


---

# Convenience methods

There are also shorthand methods for different HTTP METHODs and some other conveniences.


## request.defaults(options)

This method **returns a wrapper** around the normal request API that defaults
to whatever options you pass to it.

**Note:** `request.defaults()` **does not** modify the global request API;
instead, it **returns a wrapper** that has your default settings applied to it.

**Note:** You can call `.defaults()` on the wrapper that is returned from
`request.defaults` to add/override defaults that were previously defaulted.

For example:
```js
//requests using baseRequest() will set the 'x-token' header
var baseRequest = request.defaults({
  headers: {'x-token': 'my-token'}
})

//requests using specialRequest() will include the 'x-token' header set in
//baseRequest and will also include the 'special' header
var specialRequest = baseRequest.defaults({
  headers: {special: 'special value'}
})
```

## request.put

Same as `request()`, but defaults to `method: "PUT"`.

```js
request.put(url)
```

## request.patch

Same as `request()`, but defaults to `method: "PATCH"`.

```js
request.patch(url)
```

## request.post

Same as `request()`, but defaults to `method: "POST"`.

```js
request.post(url)
```

## request.head

Same as `request()`, but defaults to `method: "HEAD"`.

```js
request.head(url)
```

## request.del / request.delete

Same as `request()`, but defaults to `method: "DELETE"`.

```js
request.del(url)
request.delete(url)
```

## request.get

Same as `request()` (for uniformity).

```js
request.get(url)
```
## request.cookie

Function that creates a new cookie.

```js
request.cookie('key1=value1')
```
## request.jar()

Function that creates a new cookie jar.

```js
request.jar()
```

---

[back to README.md](../README.md#table-of-contents)
