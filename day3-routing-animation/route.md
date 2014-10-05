# Angular JS Routing

## ngView directive

```html
<div ng-view></div>
```
Whenever view changes following thing happen...

1. $routeChangeSuccess event is fired
2. creta a new scope
3. remove last view
4. Emits the $viewContentLoaded event
5. Run the onload attribute function, if provided

## Create Route - `$routeProvider is the key`

```javascript
angular.module('myApp', [])
	.config( '$routeProvider', function($routeProvider) {
        $routeProvider
        .when('/', {
            templateUrl: 'views/home.html',
            controller: 'HomeController'
    	});
 	});
```

## Config options of `.when()`

### controller 

controller: 'MyController'
or
controller: function($scope) {}

### template
```html
template: '<div><h2>About Page Title</h2></div>'
```
### templateUrl 

templateUrl: 'views/template_name.html'


### resolve

```javascript
resolve: {
    'myData': function($http) {
            return $http.get('/api').then(
                function success(resp) {
                    return response.data;
                }

                function error(reason) {
                    return false;
                }
            );
        }
});
```
### redirectTo: '/home'
### reloadOnSearch: true 

true by default - reload the view when route search key change
false - page won’t reload the route if the search part of the URL changes

### Handle Route Parameter - `$routeParams`

```javascript
$routeProvider
	.when('/inbox/:name', {
		controller: 'InboxController',
		templateUrl: 'views/inbox.html'
	});

// controller
angular.module("myApp")
   .controller("InboxController", function($scope, $routeParams){
   		// /inbox/all -> $routeParams = { name: "all" } 
		console.log($routeParams.name); // all
   });
```

## $location service
Provides access to current route

### path() 

http://localhost/mytest/app/#/inbox/all

$location.path() - /inbox/all
$location.path("/accounts") - change url path to `/accounts`

### replace()

If you want to redirect completely without giving the user the ability to return using the back button

```javascript
$location.path('/home').replace();
```

### absUrl() - return full url

```javascript
$location.absUrl() // http://localhost/mytest/app/#/inbox/all
```

### Others

http://localhost/mytest/app/#/inbox/all?name=hemant&loc=hyderabad

$location.hash() - inbox/all
$location.host() - localhost
$location.port() - 80 (not in url but enough smart to get it)
$location.protocol() - http
$location.url() - /inbox/all?name=hemant&loc=hyderabad
$location.search() - { name: "hemant", loc: "hydrabad"}

```javascript
// Setting search with an object
$location.search({name: 'Hemant', username: 'Hemant Singh'});
// Setting search with a string
$location.search('name=Hemnat&username=Hemant Singh');
```

## Routing Events

### $routeChangeStart

Angular broadcasts $routeChangeStart before the route changes.

```javascript
angular.module('myApp', [])
    .run(function($rootScope, $location) {
        $rootScope.$on('$routeChangeStart', function(evt, next, current) {
            
        });
    })
```
`evt- The raw Angular evt object` 
`next` - The next URL to which we are attempting to navigate
`current` - The URL that we are on before the route change


### $routeChangeSuccess

Angular broadcasts the $routeChangeSuccess event after the route dependencies have been resolved.

### $routeChangeError

Angular broadcasts the $routeChangeError event if any of the promises are rejected or fail.


```javascript
angular.module('myApp', [])
    .run(function($rootScope, $location) {
        $rootScope.$on('$routeChangeError', function(current, previous, rejection) {
            
        });
    })
```
`current` - The current route information
`previous` - The previous route information
`rejection` - The rejection promise error

### $routeUpdate

Angular broadcasts the $routeUpdate event if the reloadOnSearch property has been set to false
and we’re reusing the same instance of a controller.


## Page Reloading

The $location service does not reload the entire page; it simply changes the URL. If we need to
cause a full page reload, we have to set the location using the $window service:

```javascript
$window.location.href = "/reload/page";
```










































