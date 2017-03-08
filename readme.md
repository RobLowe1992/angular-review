# Angular Review

Let's build an Angular app from scratch!

## Outline of Steps
  0. 'Bootstrapping' our Angular app
    0. Create application files `index.html`, `script.js`
    0. Bring in Angular codebase from a CDN
    0. Define a `module` on the `angular` object
    0. Link this module using the directive `ng-app`
  0. Data & Template-binding
    0. Create an array of hard-coded data in `app.js`
    0. Define a `controller` on the `angular` object
    0. Define the `controller`'s controller function (callback function)
    0. Define a property on the ControllerFunction
      - > e.g. `this.todos = todosArray`
    0. Bind the controller to a DOM element in `index.html` using the directive `ng-controller`
    0. Use the directive `ng-repeat` to iterate over the collection in `vm.todos`.
  0. Replacing `ng-controller` with `state` from ui-router
    0. Remove `ng-controller` directive
    0. Bring in `ui.router` from a CDN
    0. Inject `ui.router` in the `modules` array of dependencies
    0. Define a `config` on the module: this both will inject `$stateProvider` and reference the `RouterFunction`
    0. Define your `RouterFunction`, injecting `$stateProvider`.
    0. Add one `state` onto `$stateProvider`
    0. Add a new template `todos-index.html`
    0. Add `ui-vew` to `<main>`

## Step 1: Setup

```bash
 $ mkdir angular_practice
 $ touch index.html app.js
```

Let's add in some HTML boilerplate!

> in `index.html`:

```HTML
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title></title>
      <!-- Bring in Angular CDN here -->
      <script src="app.js"></script>
    </head>
    <body>

    </body>
  </html>
```

Next, we'll add in the Angular codebase from a CDN, and then add a `module` to the `angular` object. If it seems odd that there is an `angular` object, think back to `jQuery`/`$`: it's the same idea of a library/framework being encapsulated in a single, imported object.

> in app.js:

```js
angular
  .module("todosAgain",[])
```

> in index.html:

```HTML
<body ng-app="todosAgain">
  {{3/0}}
</body>
```

## Step 2: Adding the Controller

Since Controllers distribute data and an application's business logic, let's add in some hard-coded data.

> in app.js

```js
let todosData = [
  {
    task: "Grind out Angular apps all day so I can stack $stacks",
    completed: true
  },
  {
    task: "Order 500lbs of Cheetos with this Angular-dev paycheck",
    completed: false
  },
  {
    task: "Monetize my meme-stream",
    completed: true
  },
  {
    task: "Create Machine Learning platform optimized for dank memes",
    completed: false
  }
]
```

Next, we will add a `controller` onto the `angular` object.

> in app.js

```js
angular
  .module("todosAgain",[])
  .controller("TodosController", TodosControllerFunction)
```

We'll also have to define the `TodosControllerFunction`.

```js
function TodosControllerFunction(){
  this.todos = todosData;
}
```

And finally let's bind the controller to a template (`index.html`). Template-binding is simply attaching a controller to a template. We will do this using `ng-controller`. Let's add in a `<main></main>` and bind the controller to it.

> in index.html

```html
<body ng-app="todosAgain">
  <!-- replace ... with the name of the controller -->
  <main ng-controller="...">

  </main>
</body>
```

> inside the <main>:

```html
<div ng-repeat="todo in vm.todos">
  {{todo.task}}
</div>
```

## Step 3: Bringing in UI Router

First, we'll grab UI Router from a CDN and bring it into our application.

Next, we'll add a `config` to the `angular` object.


```js
angular
  .module("todosAgain",[])
  .config([
    "$stateProvider",
     RouterFunction
   ])
  .controller("TodosController", TodosControllerFunction)
```

Next, we'll define `RouterFunction`:

```js
function RouterFunction($stateProvider){
  $stateProvider
    .state("todosIndex", {
      url: "/",
      templateUrl: "todos-index.html",
      controller: "TodosController",
      controllerAs: "vm"
    })
}
```

What is a state doing here? It's binding a controller to a template, and making that binding available at a route, in this case `/`.

We haven't yet made the new template `todos-index.html`, so let's do that.

Now, we'll cut this html from `index.html` and paste it to `todos-index.html`:

>
```html
<div ng-repeat="todo in vm.todos">
  {{todo.task}}
</div>
```

Then, we'll remove `ng-controller` and replace it with `ui-view`:

```HTML
<main ui-view>

</main>
```

Voila!
