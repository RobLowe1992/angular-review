# Angular Review

Let's build an Angular app from scratch!

## Troubleshooting

Refer to [this list of common errors in Angular](./angular-gotchas.md) if you get stuck.

## Outline of Steps

  1. 'Bootstrapping' our Angular app
      1. Create application files `index.html` and `script.js`
      1. Bring in Angular codebase from a CDN
      1. Define a `module` on the `angular` object
      1. Link this module using the directive `ng-app`
  1. Data & Template-binding
      1. Create an array of hard-coded data in `app.js`
      1. Define a `controller` on the `angular` object
      1. Define the `controller's`  function (callback function)
      1. Define a property on the ControllerFunction
          - e.g. `this.todos = todosArray`
      1. Bind the controller to a DOM element in `index.html` using the directive `ng-controller`
      1. Use the directive `ng-repeat` to iterate over the collection in `vm.todos`
  1. Replace `ng-controller` with `state` from ui-router
      1. Remove `ng-controller` directive
      1. Bring in `ui.router` from a CDN
      1. Inject `ui.router` in the `modules` array of dependencies
      1. Attach a `config` method to the `module`.
          - The arguments to `config` inject `$stateProvider` and reference the `RouterFunction`
      1. Define your `RouterFunction`, injecting `$stateProvider`
      1. Add one `state` onto `$stateProvider`
      1. Add a new template `todos-index.html`
      1. Add `ui-view` to `<main>`

## Step 1: Setup

```bash
 $ mkdir angular_practice
 $ touch index.html app.js
```

Let's add in some HTML boilerplate.

> in `index.html`...

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

Next, we'll add in the Angular codebase from a CDN, and then add a `module` to the `angular` object. If it seems odd that there is an `angular` object. Think back to `jQuery`/`$`. It's the same idea of a library/framework being encapsulated in a single, imported object.

> in app.js...

```js
angular
  .module("todosAgain",[])
```

> in index.html...

```HTML
<body ng-app="todosAgain">
  {{3/0}}
</body>
```

Now, open `index.html`

## Step 2: Adding the Controller

Since Controllers distribute data and an application's business logic, let's add in some hard-coded data.

> in app.js...

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

> in app.js...

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

> in index.html...

```html
<body ng-app="todosAgain">
  <!-- we're creating a new instance of our controller and saving it to a variable vm -->
  <main ng-controller="TodosController as vm">

  </main>
</body>
```

> inside the main tag...

```html
<div ng-repeat="todo in vm.todos">
  {{todo.task}}
</div>
```

## Step 3: Bringing in UI Router

First, we'll grab UI Router from a CDN and bring it into our application. UI Router is a client-side single page application routing framework that updates the browser's URL as the user navigates through the application.

Next, we'll add a `config` to the `angular` object and update our controller function. A state in Angular is basically a route: it's an umbrella term for a URL, the view associated with it, and any controllers used in that view.


```js
angular
  .module("todosAgain",["ui.router"])
  .config([
    "$stateProvider",
     RouterFunction
   ])
  .controller("TodosController",  ["$stateParams", TodosControllerFunction])

  function TodosControllerFunction($state, $stateParams){
  this.todos = todosData;
}
```

Next, we'll define `RouterFunction`...

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

We haven't yet made the new template `todos-index.html` so let's do that.

Now, we'll cut this HTML from `index.html` and paste it to `todos-index.html`.

>
```html
<div ng-repeat="todo in vm.todos">
  {{todo.task}}
</div>
```

Then, we'll remove `ng-controller` and replace it with `ui-view`...

```HTML
<main ui-view>

</main>
```

Run your server by typing `hs` in the terminal. Open your browser to `http://localhost:8080/#/` and voila!
