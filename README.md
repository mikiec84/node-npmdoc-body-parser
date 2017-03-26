# api documentation for  [body-parser (v1.17.1)](https://github.com/expressjs/body-parser#readme)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-body-parser.svg)](https://travis-ci.org/npmdoc/node-npmdoc-body-parser)
#### Node.js body parsing middleware

[![NPM](https://nodei.co/npm/body-parser.png?downloads=true)](https://www.npmjs.com/package/body-parser)

[![apidoc](https://npmdoc.github.io/node-npmdoc-body-parser/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-body-parser_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-body-parser/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-body-parser/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/expressjs/body-parser/issues"
    },
    "contributors": [
        {
            "name": "Douglas Christopher Wilson",
            "email": "doug@somethingdoug.com"
        },
        {
            "name": "Jonathan Ong",
            "email": "me@jongleberry.com",
            "url": "http://jongleberry.com"
        }
    ],
    "dependencies": {
        "bytes": "2.4.0",
        "content-type": "~1.0.2",
        "debug": "2.6.1",
        "depd": "~1.1.0",
        "http-errors": "~1.6.1",
        "iconv-lite": "0.4.15",
        "on-finished": "~2.3.0",
        "qs": "6.4.0",
        "raw-body": "~2.2.0",
        "type-is": "~1.6.14"
    },
    "description": "Node.js body parsing middleware",
    "devDependencies": {
        "eslint": "3.17.0",
        "eslint-config-standard": "7.0.0",
        "eslint-plugin-markdown": "1.0.0-beta.4",
        "eslint-plugin-promise": "3.5.0",
        "eslint-plugin-standard": "2.1.1",
        "istanbul": "0.4.5",
        "methods": "1.1.2",
        "mocha": "2.5.3",
        "supertest": "1.1.0"
    },
    "directories": {},
    "dist": {
        "shasum": "75b3bc98ddd6e7e0d8ffe750dfaca5c66993fa47",
        "tarball": "https://registry.npmjs.org/body-parser/-/body-parser-1.17.1.tgz"
    },
    "engines": {
        "node": ">= 0.8"
    },
    "files": [
        "lib/",
        "LICENSE",
        "HISTORY.md",
        "index.js"
    ],
    "gitHead": "0f1bed0543d34c8de07385157b8183509d1100aa",
    "homepage": "https://github.com/expressjs/body-parser#readme",
    "license": "MIT",
    "maintainers": [
        {
            "name": "dougwilson",
            "email": "doug@somethingdoug.com"
        }
    ],
    "name": "body-parser",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/expressjs/body-parser.git"
    },
    "scripts": {
        "lint": "eslint --plugin markdown --ext js,md .",
        "test": "mocha --require test/support/env --reporter spec --check-leaks --bail test/",
        "test-cov": "istanbul cover node_modules/mocha/bin/_mocha -- --require test/support/env --reporter dot --check-leaks test/",
        "test-travis": "istanbul cover node_modules/mocha/bin/_mocha --report lcovonly -- --require test/support/env --reporter spec --check-leaks test/"
    },
    "version": "1.17.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module body-parser](#apidoc.module.body-parser)
1.  [function <span class="apidocSignatureSpan">body-parser.</span>json (options)](#apidoc.element.body-parser.json)
1.  [function <span class="apidocSignatureSpan">body-parser.</span>raw (options)](#apidoc.element.body-parser.raw)
1.  [function <span class="apidocSignatureSpan">body-parser.</span>text (options)](#apidoc.element.body-parser.text)
1.  [function <span class="apidocSignatureSpan">body-parser.</span>urlencoded (options)](#apidoc.element.body-parser.urlencoded)



# <a name="apidoc.module.body-parser"></a>[module body-parser](#apidoc.module.body-parser)

#### <a name="apidoc.element.body-parser.json"></a>[function <span class="apidocSignatureSpan">body-parser.</span>json (options)](#apidoc.element.body-parser.json)
- description and source-code
```javascript
function json(options) {
  var opts = options || {}

  var limit = typeof opts.limit !== 'number'
    ? bytes.parse(opts.limit || '100kb')
    : opts.limit
  var inflate = opts.inflate !== false
  var reviver = opts.reviver
  var strict = opts.strict !== false
  var type = opts.type || 'application/json'
  var verify = opts.verify || false

  if (verify !== false && typeof verify !== 'function') {
    throw new TypeError('option verify must be function')
  }

  // create the appropriate type checking function
  var shouldParse = typeof type !== 'function'
    ? typeChecker(type)
    : type

  function parse (body) {
    if (body.length === 0) {
      // special-case empty json body, as it's a common client-side mistake
      // TODO: maybe make this configurable or part of "strict" option
      return {}
    }

    if (strict) {
      var first = firstchar(body)

      if (first !== '{' && first !== '[') {
        debug('strict violation')
        throw new SyntaxError('Unexpected token ' + first)
      }
    }

    debug('parse json')
    return JSON.parse(body, reviver)
  }

  return function jsonParser (req, res, next) {
    if (req._body) {
      debug('body already parsed')
      next()
      return
    }

    req.body = req.body || {}

    // skip requests without bodies
    if (!typeis.hasBody(req)) {
      debug('skip empty body')
      next()
      return
    }

    debug('content-type %j', req.headers['content-type'])

    // determine if request should be parsed
    if (!shouldParse(req)) {
      debug('skip parsing')
      next()
      return
    }

    // assert charset per RFC 7159 sec 8.1
    var charset = getCharset(req) || 'utf-8'
    if (charset.substr(0, 4) !== 'utf-') {
      debug('invalid charset')
      next(createError(415, 'unsupported charset "' + charset.toUpperCase() + '"', {
        charset: charset
      }))
      return
    }

    // read
    read(req, res, next, parse, debug, {
      encoding: charset,
      inflate: inflate,
      limit: limit,
      verify: verify
    })
  }
}
```
- example usage
```shell
...
The 'bodyParser' object exposes various factories to create middlewares. All
middlewares will populate the 'req.body' property with the parsed body, or an
empty object ('{}') if there was no body to parse (or an error was returned).

The various errors returned by this module are described in the
[errors section](#errors).

### bodyParser.json(options)

Returns middleware that only parses 'json'. This parser accepts any Unicode
encoding of the body and supports automatic inflation of 'gzip' and 'deflate'
encodings.

A new 'body' object containing the parsed data is populated on the 'request'
object after the middleware (i.e. 'req.body').
...
```

#### <a name="apidoc.element.body-parser.raw"></a>[function <span class="apidocSignatureSpan">body-parser.</span>raw (options)](#apidoc.element.body-parser.raw)
- description and source-code
```javascript
function raw(options) {
  var opts = options || {}

  var inflate = opts.inflate !== false
  var limit = typeof opts.limit !== 'number'
    ? bytes.parse(opts.limit || '100kb')
    : opts.limit
  var type = opts.type || 'application/octet-stream'
  var verify = opts.verify || false

  if (verify !== false && typeof verify !== 'function') {
    throw new TypeError('option verify must be function')
  }

  // create the appropriate type checking function
  var shouldParse = typeof type !== 'function'
    ? typeChecker(type)
    : type

  function parse (buf) {
    return buf
  }

  return function rawParser (req, res, next) {
    if (req._body) {
      debug('body already parsed')
      next()
      return
    }

    req.body = req.body || {}

    // skip requests without bodies
    if (!typeis.hasBody(req)) {
      debug('skip empty body')
      next()
      return
    }

    debug('content-type %j', req.headers['content-type'])

    // determine if request should be parsed
    if (!shouldParse(req)) {
      debug('skip parsing')
      next()
      return
    }

    // read
    read(req, res, next, parse, debug, {
      encoding: null,
      inflate: inflate,
      limit: limit,
      verify: verify
    })
  }
}
```
- example usage
```shell
...

##### verify

The 'verify' option, if supplied, is called as 'verify(req, res, buf, encoding)',
where 'buf' is a 'Buffer' of the raw request body and 'encoding' is the
encoding of the request. The parsing can be aborted by throwing an error.

### bodyParser.raw(options)

Returns middleware that parses all bodies as a 'Buffer'. This parser
supports automatic inflation of 'gzip' and 'deflate' encodings.

A new 'body' object containing the parsed data is populated on the 'request'
object after the middleware (i.e. 'req.body'). This will be a 'Buffer' object
of the body.
...
```

#### <a name="apidoc.element.body-parser.text"></a>[function <span class="apidocSignatureSpan">body-parser.</span>text (options)](#apidoc.element.body-parser.text)
- description and source-code
```javascript
function text(options) {
  var opts = options || {}

  var defaultCharset = opts.defaultCharset || 'utf-8'
  var inflate = opts.inflate !== false
  var limit = typeof opts.limit !== 'number'
    ? bytes.parse(opts.limit || '100kb')
    : opts.limit
  var type = opts.type || 'text/plain'
  var verify = opts.verify || false

  if (verify !== false && typeof verify !== 'function') {
    throw new TypeError('option verify must be function')
  }

  // create the appropriate type checking function
  var shouldParse = typeof type !== 'function'
    ? typeChecker(type)
    : type

  function parse (buf) {
    return buf
  }

  return function textParser (req, res, next) {
    if (req._body) {
      debug('body already parsed')
      next()
      return
    }

    req.body = req.body || {}

    // skip requests without bodies
    if (!typeis.hasBody(req)) {
      debug('skip empty body')
      next()
      return
    }

    debug('content-type %j', req.headers['content-type'])

    // determine if request should be parsed
    if (!shouldParse(req)) {
      debug('skip parsing')
      next()
      return
    }

    // get charset
    var charset = getCharset(req) || defaultCharset

    // read
    read(req, res, next, parse, debug, {
      encoding: charset,
      inflate: inflate,
      limit: limit,
      verify: verify
    })
  }
}
```
- example usage
```shell
...

##### verify

The 'verify' option, if supplied, is called as 'verify(req, res, buf, encoding)',
where 'buf' is a 'Buffer' of the raw request body and 'encoding' is the
encoding of the request. The parsing can be aborted by throwing an error.

### bodyParser.text(options)

Returns middleware that parses all bodies as a string. This parser supports
automatic inflation of 'gzip' and 'deflate' encodings.

A new 'body' string containing the parsed data is populated on the 'request'
object after the middleware (i.e. 'req.body'). This will be a string of the
body.
...
```

#### <a name="apidoc.element.body-parser.urlencoded"></a>[function <span class="apidocSignatureSpan">body-parser.</span>urlencoded (options)](#apidoc.element.body-parser.urlencoded)
- description and source-code
```javascript
function urlencoded(options) {
  var opts = options || {}

  // notice because option default will flip in next major
  if (opts.extended === undefined) {
    deprecate('undefined extended: provide extended option')
  }

  var extended = opts.extended !== false
  var inflate = opts.inflate !== false
  var limit = typeof opts.limit !== 'number'
    ? bytes.parse(opts.limit || '100kb')
    : opts.limit
  var type = opts.type || 'application/x-www-form-urlencoded'
  var verify = opts.verify || false

  if (verify !== false && typeof verify !== 'function') {
    throw new TypeError('option verify must be function')
  }

  // create the appropriate query parser
  var queryparse = extended
    ? extendedparser(opts)
    : simpleparser(opts)

  // create the appropriate type checking function
  var shouldParse = typeof type !== 'function'
    ? typeChecker(type)
    : type

  function parse (body) {
    return body.length
      ? queryparse(body)
      : {}
  }

  return function urlencodedParser (req, res, next) {
    if (req._body) {
      debug('body already parsed')
      next()
      return
    }

    req.body = req.body || {}

    // skip requests without bodies
    if (!typeis.hasBody(req)) {
      debug('skip empty body')
      next()
      return
    }

    debug('content-type %j', req.headers['content-type'])

    // determine if request should be parsed
    if (!shouldParse(req)) {
      debug('skip parsing')
      next()
      return
    }

    // assert charset
    var charset = getCharset(req) || 'utf-8'
    if (charset !== 'utf-8') {
      debug('invalid charset')
      next(createError(415, 'unsupported charset "' + charset.toUpperCase() + '"', {
        charset: charset
      }))
      return
    }

    // read
    read(req, res, next, parse, debug, {
      debug: debug,
      encoding: charset,
      inflate: inflate,
      limit: limit,
      verify: verify
    })
  }
}
```
- example usage
```shell
...

##### verify

The 'verify' option, if supplied, is called as 'verify(req, res, buf, encoding)',
where 'buf' is a 'Buffer' of the raw request body and 'encoding' is the
encoding of the request. The parsing can be aborted by throwing an error.

### bodyParser.urlencoded(options)

Returns middleware that only parses 'urlencoded' bodies. This parser accepts
only UTF-8 encoding of the body and supports automatic inflation of 'gzip'
and 'deflate' encodings.

A new 'body' object containing the parsed data is populated on the 'request'
object after the middleware (i.e. 'req.body'). This object will contain
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
