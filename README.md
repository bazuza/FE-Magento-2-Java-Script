[![picture alt](https://raw.githubusercontent.com/bazuza/FE-Magento-2-Guide/master/logo-m2-fe.png "Main page")](https://github.com/bazuza/FE-Magento-2-Guide)

## Create custom js and including third-party library

### Create `theme.js`
1. Create `theme.js` within the theme directory `<Vendor>/<theme>/web/js/theme.js` with this content:
```
define([
  "jquery"
], 
function($) {
  "use strict";

  // Here your custom code...

});
```
2. Declare `theme.js` with a `requirejs-config.js` file

Create a `requirejs-config.js` file within the theme directory: `<Vendor>/<theme><theme>/requirejs-config.js` with this content:
```
var config = {

  // When load 'requirejs' always load the following files also
  deps: [
    "js/theme"
  ]

};
```
`js/theme` is the path to our custom `theme.js`. The `.js` extension is not required.

Custom `requirejs-config.js` will be merged with other `requirejs-config.js` defined in Magento.

RequireJS will load our `theme.js` file, on each page, resolving dependencies and loading files in an async way.

### Optional: Including third-party library
1. Add the library in `web/js`: `<Vendor>/<theme>/web/js/vendor/jquery/library.js`
2. Open `requirejs-config.js` and add this content:
```
var config = {

  deps: [
    "js/theme"
  ],

  // Paths defines associations from library name (used to include the library,
  // for example when using "define") and the library file path.
  paths: {
    'library': 'js/vendor/jquery/library',
  },

  // Shim: when you're loading your dependencies, requirejs loads them all
  // concurrently. You need to set up a shim to tell requirejs that the library
  // (e.g. a jQuery plugin) depends on another already being loaded (e.g. depends
  // from jQuery).
  // Exports: if the library it's not AMD aware, you need to tell requirejs what 
  // to look to know the script loaded correctly. You can do this with an 
  // "exports" entry in your shim. The value must be a variable defined within
  // the library.
  shim: {
    'library': {
      deps: ['jquery'],
      exports: 'jQuery.fn.library',
    }
  }

};
```
3. Add the dependency within `theme.js`:
```
define([
  'jquery',
  'library'
], 
function($) {

  // ...

});
```

#### Your theme directory structure now
```
app/design/<area>/<Vendor>/<theme>/
├── web/
│ ├── css/
│ │ ├── source/ 
│ ├── fonts/
│ ├── images/
│ ├── js/
│ │ ├── theme.js
│ │ ├── vendor/
│ │ │ ├── jquery/
│ │ │ │ ├── library.js
```

## Resource:
* [Create cusom js and include third-party library](http://devdocs.magento.com/guides/v2.0/javascript-dev-guide/javascript/js-resources.html)
