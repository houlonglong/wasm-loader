# WASM Binary Module loader for Webpack

A simple `.wasm` binary file loader for Webpack. Import your wasm modules directly into your bundle as Constructors which return `WebAssembly.Instance`. This avoids the need to use fetch and parse for your wasm files. Imported wasm files
are converted to Uint8Arrays and become part of the full JS bundle!

## Install

Install package:
`npm install --save wasm-loader`

# Usage

Edit webpack.config.js:
```
  loaders: [
    {
      test: /\.wasm$/,
      loaders: ['wasm-loader']
    }
  ]
```

Include wasm from your code:

```
import Counter from 'wasm/counter';

const counter = new Counter();
console.log(counter.exports.count()); // 0
console.log(counter.exports.count()); // 1
console.log(counter.exports.count()); // 2
```

The default export from wasm module is a constructor which returns a [WebAssembly.Instance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/Instance). `deps` can be passed in to
override defaults. For example

```
const customCounter = new Counter({
  'global': {},
  'env': {
    'memory': new WebAssembly.Memory({initial: 100, limit: 1000}),
    'table': new WebAssembly.Table({initial: 0, element: 'anyfunc'})
  }
});
```

*Default deps are:*
```
{
  'global': {},
  'env': {
    'memory': new Memory({initial: 10, limit: 100}),
    'table': new Table({initial: 0, element: 'anyfunc'})
  }
}
```
