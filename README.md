# npmdoc-body-parser

#### api documentation for  [body-parser (v1.17.1)](https://github.com/expressjs/body-parser#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-body-parser.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-body-parser) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-body-parser.svg)](https://travis-ci.org/npmdoc/node-npmdoc-body-parser)

#### Node.js body parsing middleware

[![NPM](https://nodei.co/npm/body-parser.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/body-parser)

- [https://npmdoc.github.io/node-npmdoc-body-parser/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-body-parser/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-body-parser/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-body-parser/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-body-parser/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-body-parser/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/expressjs/body-parser/issues"
    },
    "contributors": [
        {
            "name": "Douglas Christopher Wilson"
        },
        {
            "name": "Jonathan Ong",
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
            "name": "dougwilson"
        }
    ],
    "name": "body-parser",
    "optionalDependencies": {},
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



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
