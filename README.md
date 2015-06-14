### This is now the official Vue loader, the latest version can be found at https://github.com/vuejs/vue-loader

# vue-multi-loader

> Vue.js component loader for [Webpack](http://webpack.github.io), using Webpack loaders for the parts.

It allows you to write your components in this format:

``` html
// app.vue
<style>
  .red {
    color: #f00;
  }
</style>

<template>
  <h1 class="red">{{msg}}</h1>
</template>

<script>
  module.exports = {
    data: function () {
      return {
        msg: 'Hello world!'
      }
    }
  }
</script>
```

You can also mix preprocessor languages in the component file:

``` html
// app.vue
<style lang="stylus">
.red
  color #f00
</style>

<template lang="jade">
h1(class="red") {{msg}}
</template>

<script lang="coffee">
module.exports =
  data: ->
    msg: 'Hello world!'
</script>
```

And you can import using the `src` attribute (note that there's no need for a `lang` attribute here, as Webpack will
be used to determine which loader applies):

``` html
<style src="style.styl"></style>
```

## Usage

Config Webpack:

``` js
// webpack.config.js
module.exports = {
  entry: "./main.js",
  output: {
    filename: "build.js"
  },
  module: {
    loaders: [
      { test: /\.vue$/, loader: "vue-multi-loader" },
    ]
  }
}
```

And this is all you need to do in your main entry file:

``` js
// main.js
var Vue = require('vue')
var appOptions = require('./app.vue')
var app = new Vue(appOptions).$mount('#app')
```

## Loader configuration

By default, `vue-multi-loader` will try to use the loader with the same name as
the `lang` attribute, but you can configure which loader should be used.

For example, to extract out the generated css into a separate file,
use this configuration:

``` js
// webpack.config.js
var ExtractTextPlugin = require("extract-text-webpack-plugin");
var vue = require("vue-multi-loader");

module.exports = {
  entry: "./main.js",
  output: {
    filename: "build.js"
  },
  module: {
    loaders: [
      {
        test: /\.vue$/, loader: vue.withLoaders({
          css: ExtractTextPlugin.extract("css"),
          stylus: ExtractTextPlugin.extract("css!stylus")
        })
      },
    ]
  },
  plugins: [
    new ExtractTextPlugin("[name].css")
  ]
}
```
