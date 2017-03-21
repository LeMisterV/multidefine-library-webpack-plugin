# Multidefine Library Webpack Plugin

This <s>hack</s> plugin allow webpack 2 to bundle your code to an amd library with multiple entries in a same asset.

## Requirements

- node >= 6.9.4
- webpack >= 2.2.0

## Installation

```bash
npm install multidefine-library-webpack-plugin --save
```

Or add it to your package.json

## Usage

Add the plugin in your `webpack.config.js`:

```javascript
'use strict';

const MultidefineLibraryWebpackPlugin = require('multidefine-library-webpack-plugin');

module.exports = {
  entry: ['a.js', 'b.js'],
  output: {
    path: './build',
    filename: '[name].js'
  },
  plugins: [
    new MultidefineLibraryWebpackPlugin([{
      path: 'a.js',
      name: 'A'
    }, {
      path: 'b.js',
      name: 'B',
      type: 'amd'
    }])
  ]
};
```

### First parameter

The plugin take an array of objects as first parameter. Each object will represent an entry point of your asset:
```javascript
[{
  path: "a.js", // path to file that you want to make requireable
  name: "A", // Define name of the module
  [type: "amd"] // Specifiy if the origin module in in amd or commonjs, default value is commonjs
  [aliases: ["alias1", "alias2"]] // alternate deprecated names exposed for this module
  [deprecated: true/false] // If deprecated, a global method "deprecated" is called is this module is used
}]
```
This plugin support currently only `amd` and `commonjs` source module.

Then you will be able to require these module:

```javascript
require(['A', 'B'], function (A, B) {
  // A and B are available !
});
```

### Second parameter

The plugin take an options object as second parameter.
```javascript
    new MultidefineLibraryWebpackPlugin(modulesToExpose, {
      deprecationMethodName: 'deprecated' // global method name to call to notify deprecated usage
    })

```
