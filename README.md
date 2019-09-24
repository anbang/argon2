[![Build Status](https://travis-ci.org/canonchain/argon2-wasm.svg?branch=master)](https://travis-ci.org/canonchain/argon2-wasm)

# argon2-wasm

https://github.com/P-H-C/phc-winner-argon2 compiled to WebAssembly using Emscripten and optimized for small size

argon2 run as WebAssembly.

## usage

install:

```js
npm install git+https://github.com/canonchain/argon2-wasm.git
```

Add script:

```js
const argon2 = require("argon2-wasm");
```

Get hash:

```js
argon2.hash({ pass: 'password', salt: 'somesalt' })
    .then(h => console.log(h.hash, h.hashHex, h.encoded))
    .catch(e => console.error(e.message, e.code))
```

Verify the encoded hash:

```js
argon2.verify({ pass: 'password', encoded: 'enc-hash' })
    .then(() => console.log('OK'))
    .catch(e => console.error(e.message, e.code))
```

Bring your own bundler and promise polyfill.
Other parameters:

```js
argon2.hash({
    // required
    pass: 'password',
    salt: 'salt',
    // optional
    time: 1, // the number of iterations
    mem: 1024, // used memory, in KiB
    hashLen: 24, // desired hash length
    parallelism: 1, // desired parallelism (will be computed in parallel only for PNaCl)
    type: argon2.ArgonType.Argon2d, // or argon2.ArgonType.Argon2i
    distPath: '' // asm.js script location, without trailing slash
})
// result
.then(res => {
    res.hash // hash as Uint8Array
    res.hashHex // hash as hex-string
    res.encoded // encoded hash, as required by argon2
})
// or error
.catch(err => {
    err.message // error message as string, if available
    err.code // numeric error code
})
```



## Building

Prerequesties:

- emscripten with WebAssembly support (https://webassembly.org/getting-started/developers-guide/)
- CMake
