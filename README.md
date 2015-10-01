bootstrap-webpack
=================

bootstrap package for webpack.

This is the `less` version. If you are looking for the `sass` version, refer to [bootstrap-sass-loader](https://github.com/justin808/bootstrap-sass-loader).


Usage
-----

Bootstrap use some fonts. You need to configure the correct loaders in your `webpack.config.js`. Example:

``` javascript
module.exports = {
  module: {
    loaders: [
      // the url-loader uses DataUrls.
      // the file-loader emits files.
      { test: /\.(woff|woff2)$/,  loader: "url-loader?limit=10000&mimetype=application/font-woff" },
      { test: /\.ttf$/,    loader: "file-loader" },
      { test: /\.eot$/,    loader: "file-loader" },
      { test: /\.svg$/,    loader: "file-loader" }
    ]
  }
};
```

### Complete Bootstrap

To use the complete bootstrap package including styles and scripts with the default settings:

``` javascript
require("bootstrap-webpack");
```

### Custom configuration

You can configurate bootstrap-webpack with two configuration files:

* `bootstrap.config.js`
* `bootstrap.config.less`

Add both files next to each other to your project. And:

``` javascript
require("bootstrap-webpack!./bootstrap.config.js");
```

Or simple add it as entry point to your `webpack.config.js`:

``` javascript
module.exports = {
  entry: [
    "bootstrap-webpack!./bootstrap.config.js",
    "your-existing-entry-point"
  ]
};
```

### Using with other js loaders.

If you are using other loaders for all js files`(test: /\.js$/)`, this might interfere with bootstrap-webpack.
You can override the configuration loader order in the module request to suit special cases.

* adding ! to a request will disable configured preLoaders
``` javascript
require("!bootstrap-webpack!./bootstrap.config.js")
```
* adding !! to a request will disable all loaders specified in the configuration
``` javascript
require("!!bootstrap-webpack!./bootstrap.config.js")
```
* adding -! to a request will disable configured preLoaders and loaders but not the postLoaders
``` javascript
require("-!bootstrap-webpack!./bootstrap.config.js")
```

Check details in [`webpack loader order`](https://webpack.github.io/docs/loaders.html)

You can also change your configuration, so that other loaders are not applied to bootstrap.

1. Use `exclude` option of the module.loaders section of the config.
2. Adjust the regex in `test` option of the module loaders to prevent matching all the js files to which the loaders are applied.

See the explanation of different module options under [`module.loaders`](http://webpack.github.io/docs/configuration.html)

#### `bootstrap.config.js`

Example:

``` javascript
module.exports = {
  scripts: {
    // add every bootstrap script you need
    'transition': true
  },
  styles: {
    // add every bootstrap style you need
    "mixins": true,

    "normalize": true,
    "print": true,

    "scaffolding": true,
    "type": true,
  }
};
```

#### `bootstrap.config.less`

Write less code. I. e. overwrite the default colors or sizes.

Example:

``` less
@font-size-base:          24px;

@btn-default-color:              #444;
@btn-default-bg:                 #eee;
```

### extract-text-webpack-plugin

Configure post style loaders in `bootstrap.config.js`.

Example:

``` javascript
module.exports = {
  postStyleLoaders: [
    require.resolve('extract-text-webpack-plugin/loader.js') + '?{"omit":1,"extract":true,"remove":true}'
  ],
  scripts: {
    ...
  },
  styles: {
    ...
  }
};
```

Install `extract-text-webpack-plugin` before using this configuration.
