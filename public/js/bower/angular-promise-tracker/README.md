angular-promise-tracker
=======================

* [Demo](http://plnkr.co/edit/3uAe0NdXLz1lCYlhpaMp?p=preview)
* [Help](https://github.com/ajoslin/angular-promise-tracker/wiki)
* [Changes](https://github.com/ajoslin/angular-promise-tracker/tree/master/CHANGELOG.md)
* [License](#license)


## Eh, what?

So you're building your angular app.  And you want a loading spinner.

You've tried the [normal solution](http://jsfiddle.net/zdam/dBR2r/) (or maybe you haven't), and it has problems.  It presents a loading spinner on *every request*!

But you don't want the same global loading spinner whenever any request happens anywhere. That just won't work!

Instead, you want different indicators while different types of request are loading.  You want one spinner while you're fetching data having to do with a user's pizza order, one while fetching user's profile data, and maybe another for some random service you have that returns a promise. All these on different parts of the UI.  Heck, maybe you don't even want a spinner.  You just want to know while http requests of some type are pending.

Well, [sigh no more](http://www.youtube.com/watch?v=eltHv58l8ig) my dear friend, your troubles are over.


* Throw a promiseTracker onto your scope

  ```js
  function MyCtrl($scope, promiseTracker) {
      $scope.pizzaTracker = promiseTracker('pizza');
  }
  ```

* Do some requests, and in their config add in a little option called `tracker`

  ```js
  $http.get('/pizzaFlavor', { tracker: 'pizza' });
  $http.get('/pizzaType', { tracker: 'pizza' });
  $http.get('/pizzaCrust', { tracker: 'pizza' });
  ```

* Now the awesomes happen: `pizzaTracker.active()` will be true whenever any request with `tracker: 'pizza'` is waiting for response!
 

  ```html
  <div ng-show="pizzaTracker.active()" style="background: pink;">
      Loading some pizza data for ya, sir! ...
  </div>
  ```

* But wait, there's more! You can also catch cool events when stuff happens on any pizza promise...

  ```js
  $scope.pizzaTracker.on('error', function(response) {
      $scope.pizzaError = "Uh oh, some sort of pizza error happened! " + response.data;
  });
  $http.get('/pizzaError', { tracker: 'pizza' });
  ```
  ```html
  <b ng-show="pizzaError" style="color:red;">!! {{pizzaError}} !!</b>
  ```

You can catch any of these events: `'error'`, `'success'`, `'start'`, `'done'`.  Hopefully they all make sense.

*  Oh, and did I mention... you can attach any old promise to your pizza tracker.  Not just http requests!

  ```js
  var myPizzaPromise = $q.defer();
  $scope.pizzaTracker.addPromise(myPizzaPromise.promise);
  ```

-

## Development

* Install karma & grunt with `npm install -g karma grunt-cli` to build & test
* Install local dependencies with `bower install && npm install`
* Run `grunt` to lint, test, build the code, and build the docs site

## <a id="license"></a>License

> <a rel="license" href="http://creativecommons.org/publicdomain/mark/1.0/"> <img src="http://i.creativecommons.org/p/mark/1.0/80x15.png" style="border-style: none;" alt="Public Domain Mark" /> </a> <span property="dct:title">angular-promise-tracker</span> by <a href="http://andybam.com" rel="dct:creator"><span property="dct:title">Andy Joslin</span></a> is free of known copyright restrictions.
