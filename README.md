
# Request - Simplified HTTP client

[![npm package](https://nodei.co/npm/request.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/request/)

[![Build status](https://img.shields.io/travis/request/request/master.svg?style=flat-square)](https://travis-ci.org/request/request)
[![Coverage](https://img.shields.io/codecov/c/github/request/request.svg?style=flat-square)](https://codecov.io/github/request/request?branch=master)
[![Coverage](https://img.shields.io/coveralls/request/request.svg?style=flat-square)](https://coveralls.io/r/request/request)
[![Dependency Status](https://img.shields.io/david/request/request.svg?style=flat-square)](https://david-dm.org/request/request)
[![Known Vulnerabilities](https://snyk.io/test/npm/request/badge.svg?style=flat-square)](https://snyk.io/test/npm/request)
[![Gitter](https://img.shields.io/badge/gitter-join_chat-blue.svg?style=flat-square)](https://gitter.im/request/request?utm_source=badge)


## Super simple to use

Request is designed to be the simplest way possible to make http calls. It supports HTTPS and follows redirects by default.

```js
var request = require('request');
request('http://www.google.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage.
  }
})
```

## Table of contents

- [Streaming](doc/guides/streaming.md)
- [Forms](doc/guides/forms.md)
- [HTTP Authentication](doc/guides/http-authentication.md)
- [Custom HTTP Headers](doc/guides/custom-http-headers.md)
- [OAuth Signing](doc/guides/oauth-signing.md)
- [Proxies](doc/guides/proxies.md)
- [Unix Domain Sockets](doc/guides/unix-domain-sockets.md)
- [TLS/SSL Protocol](doc/guides/tls-ssl-protocol.md)
- [Support for HAR 1.2](doc/guides/support-for-har-1.2.md)
- [Timeouts](doc/guides/timeouts.md)
- [**All Available Options**](doc/api.md)

Request also offers [convenience methods](doc/guides/api.md) like
`request.defaults` and `request.post`, and there are
lots of [usage examples](doc/examples.md) and several
[debugging techniques](doc/guides/debugging.md).
