---
title: Plugins
---

How do I use a plugin ? - command line
--------------------------------------

If you are using lessc, the first thing you need to do is install that plugin. We reccommend the plugin starts "less-plugin" though that isn't required. For the clean css plugin you would install `npm install less-plugin-clean-css`.

To use the plugin, if you specify a unrecognised option, we attempt to load that, for example
```
lessc --clean-css="advanced"
```

Will use the plugin you just installed. You can also be more direct, for example

```
lessc --plugin=path_to_plugin=options
```

Using a plugin in code
----------------------

In Node, require the plugin and pass it to less in an array as an option plugins. E.g.

```
var myPlugin = require("my-plugin");
less.render(myCSS, { plugins: [myPlugin] })
   .then(function(output) {
    },
    function(error) {
    });
```

In the browser
-------------------

Plugin authors should provide a javascript file, just include that in the page *before* the less.js script.

```
<script src="plugin.js"></script>
<script>
less = { 
    plugins: [plugin]
};
</script>  
<script src="less.min.js"></script>
```

List of less plugins
--------------------

### Postprocessor/Feature Plugins:
 - [Autoprefixer plugin to add backwards compatibility to your css](https://github.com/less/less-plugin-autoprefix)
 - [CSScomb plugin for less.js](https://github.com/bassjobsen/less-plugin-csscomb/)
 - [Clean CSS plugin to compress less](https://github.com/less/less-plugin-clean-css)
 - [Compresses the css output from Less using csswring](https://github.com/bassjobsen/less-plugin-csswring)
 - [Generate left-to-right (LTR) or right-to-left (RTL) CSS from Less using css-flip](https://github.com/bassjobsen/less-plugin-css-flip)
 - [Group CSS Media Queries - group CSS media queries](https://github.com/bassjobsen/less-plugin-group-css-media-queries)
 - [Inline urls - converts `url()` to a call to `data-uri()`](https://github.com/less/less-plugin-inline-urls)
 - [Postprocess Less using pleeease](https://github.com/bassjobsen/less-plugin-pleeease)
 - [Reverses Less code from ltr to rtl](https://github.com/less/less-plugin-rtl)
 
### Framework/Library Wrappers and Preprocessors:
 - [Bootstrap for less.js](https://github.com/bassjobsen/less-plugin-bootstrap/)
 - [Bower Resolve - import from a Bower package](https://github.com/Mercateo/less-plugin-bower-resolve)
 - [Cardinal CSS for less.js](https://github.com/bassjobsen/less-plugin-cardinal)
 - [Flexbox grid from flexboxgrid.com for less.js](https://github.com/bassjobsen/less-plugin-flexboxgrid) 
 - [Flexible Grid System (flexible.gs) for less.js ](https://github.com/bassjobsen/less-plugin-flexiblegs)
 - [Ionic for less.js](https://github.com/bassjobsen/less-plugin-ionic)
 - [Lesshat for less.js](https://github.com/bassjobsen/less-plugin-lesshat/)
 - [Npm Import - import from a sub npm repository](https://github.com/less/less-plugin-npm-import)
 - [Skeleton for less.js](https://github.com/bassjobsen/less-plugin-skeleton)

### Function Libraries:
 - [advanced-color-functions. Adds some advanced colour functions that helps in finding more contrasting colors](https://github.com/less/less-plugin-advanced-color-functions/)
 - [cubehelix. The `cubehelix(y,a,b,t)` function returns a color between the two colors a and b, using a gamma correction value of 1](https://github.com/bassjobsen/less-plugin-cubehelix).
 - [less-plugin-lists. Lists/arrays manipulation functions library](https://github.com/seven-phases-max/less-plugin-lists)


For plugin authors
--------------------------

Less supports some entry points that allow an author to integrate with less. We may add some more in the future.

The plugin itself has a very simple signtaure, like this
```
{
    install: function(less, pluginManager) {
    },
    minVersion: [2, 0, 0] /* optional */
}
```
So, the plugin gets the less object, which in v2 has more classes on it (making it easy to extend), a plugin manager which provides some hooks to add visitors, file managers and post processors.

If your plugin supports lessc, there are a few more details and the signature looks like this

```
{
    install: function(less, pluginManager) {
    },
    setOptions: function(argumentString) { /* optional */
    },
    printUsage: function() { /* optional */
    },
    minVersion: [2, 0, 0] /* optional */
}
```
The additions are the setOptions function which passes the string the user enters when specifying your plugin and also the printUsage function which you should use to explain your options and how the plugin works.

Here are some example repos showing the different plugin types
post-processor: https://github.com/less/less-plugin-clean-css
visitor: https://github.com/less/less-plugin-inline-urls
file-manager: https://github.com/less/less-plugin-npm-import

Note: Plugins are different from creating a version of less for a different environment but they do have similarities, for example node provides 2 file managers by default and browser provides one and that is the main step in getting less to run within a specific environment. The plugin allows you to add file managers.
