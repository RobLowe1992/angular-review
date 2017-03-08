# So you're getting an Angular error?

Here are some common reasons:

## Miscellaneous Syntax Errors

### Not having caching disabled in Chrome
![Caching disabling gif](http://i.imgur.com/p2TZixz.gif)

### Dependencies not being inside an array
```js
angular
.module("myModule", [
  "ngResource"
]);
// This is good

angular
.module("myModule", []);
  "ngResource"
// This is bad
```

### Using curlies where you need parentheses
```js
someState({id: 42})
// This is good

someState{{id: 42}}
// This is bad
```

## Script Tags & Dependencies

### Putting your dependencies in the function in a different order than the array
```js
angular
.module("myModule")
.controller("myController", [
  "dependencyOne",
  "dependencyTwo",
  ControllerFunction
]);

function ControllerFunction(dependencyTwo, dependencyOne){
  // This is bad
}
function ControllerFunction(dependencyOne, dependencyTwo){
  // This is good
}
```

### Malformed script tags

```html
<script></script>
<!-- This is good -->

<script />
<!-- This is bad -->

<script>
<!-- This is bad -->

```

### Missing a `<script>` tag for a library
`ui.router` needs to be loaded from a CDN

### Missing `<script>` tags for some of your `.js` files
Every `.js` file should have a `<script>` tag in your main `index.html`

### `<script>` tags in the wrong order
1. Libraries come first (`angular.min.js`, `angular-resource.js`)
- Your main `app.js`

If your main module contains sub-modules:
- Your first submodule (`grumbles.js`)
- Factories and Services
- Controllers

## `ui-router errors`

### Writing `ui-router` instead of `ui.router`

```js
angular
.module("Facebook7", [
  "ui-router", //this is wrong
  "ngResource"
])
```

```js
angular
.module("Facebook7", [
  "ui.router", //this is right
  "ngResource"
])
```

### Forgetting to add a `ui-view` directive

> I'm not seeing anything show up and I'm using ui.router!

```html
<main data-ui-view></main>
```

### Writing `templateURL` instead of `templateUrl` in your router function

> Example:

```js
  function Router($stateProvider, $locationProvider){
    $locationProvider.html5Mode(true);
    $stateProvider
    .state("index", {
      url: "/goatpeople",
      templateURL: "/assets/html/goatpeople-index.html", // Danger ZOONNNEE
      controller: "Index",
      controllerAs: "IndexVm"
    })
    .state("show", {
      url: "/goatpeople/:id",
      templateUrl: "/assets/html/goatpeople-show.html",
      controller: "Show",
      controllerAs: "ShowVm"
    });
  }
```

## IIFE / Scope Errors

### IIFEs are missing trailing parentheses
```js
(function(){
  // This is good
}())

(function(){
  // This is good
})()

(function(){
  // This is bad
})
```
