# Angular JS Services in Depth

Services are singleton objects that are instantiated only once per app (by the $injector) and lazyloaded (created only when necessary)


## Create Angular JS Service

We can create service with five different methods ...
• factory()
• service()
• constant()
• value()
• provider()

## 1. factory

```javascript
angular.module("myApp")
	.factory('myService', function(){
	    return {
	        name: "Hemant",
	        loc: "Hyderabad"
	    };
	})

// Using Service

angular.module("myApp")
	.controller('HomeCtrl', function(myService){
		$scope.user = myService;	
	});
```

## 2. service

```javascript
angular.module("myApp")
	.factory('myService', function(){
        this.users =  {
	        name: "Hemant",
	        loc: "Hyderabad"
	    };
	});
```

## 3. provider

If we want to be able to configure the service in the config() function, we must use provider()
to define our service.

> Second argument could be object/function/array

```javascript
angular.module("myApp")
	.provider('myService', {
        $get: function(){
	        return {
		        name: "Hemant",
		        loc: "Hyderabad"
	    	}
	    };
	});
```

### 4. constant

- can be injected in config
- 2nd argument (value) can not be object or function

```javascript
angular.module('myApp', [])
	.constant('apiKey', '123123123')
	.config(function(apiKey) {
	// The apiKey here will resolve to 123123123
});
```
### 5. value

- can not be injected in config
- 2nd argument (value) could be object or function

```javascript
angular.module('myApp')
	.value('apiKey', '123123123')
```

## Server-Side Communication

### $http service

```javascript
$http({
		method: 'GET',
		url: '/api/users.json'
	})
	.success(function(data, status, headers, config) {
	
	})
	.error(function(data, status, headers, config) {
		
});

or

$http({
		method: 'GET',
		url: '/api/users.json'
	})
	.then(function(resp) {
		// resp is a response object
	}, function(resp) {
		// resp with error
});

```

## Shortcut $http Methods

```javascript
$http.get('/api/users', config);
$http.jsonp('/api/users?callback=JSON_CALLBACK', config);
$http.post('/api/users', data, config);
$http.put('/api/users', data, config);
$http.delete('/api/users/1', config);
```

> config (optional object)
```
{
	method: ‘GET’, ‘DELETE’, ‘HEAD’, ‘JSONP’, ‘POST’, ‘PUT’
	url: "/users" (absolute/relative)
	params: {name: "hemant"}  (string/object) // ?name=hemant
	data: {name: "XXX", age: 20, loc: "Hyd"} (sting/object)
	headers: (object)
	xsrfHeaderName: (string)
	xsrfCookieName: (string)
	transformRequest: (function/array of functions)
	transformResponse: (function/array of functions)
	cache: (boolean/Cache object)
	timeout: (number/promise)
	withCredentials: (boolean)
	responseType: (string)

}	
```

> transformRequest
	- takes HTTP request body and headers and returns their transformed versions.
	- We generally use it to serialize data before it is sent to the server.

	function(data, headers){ }

> transformResponse
	- takes HTTP response body and headers and returns their transformed versions.
	- We generally use it to deserialize data after it has received returned data.

	function(data, headers){ }

> cache
	If this boolean value is true, then the default $http cache will cache the GET request. 
	If it is a cache object built with $cacheFactory, then Angular will use this new cache to cache the response.

> responseType
	• ”” (string – default)
	• “document” (HTTP document)
	• “json” (JSON object parsed from a JSON string)
	• “text” (string)	

## Respponse Object ( 4 argument ) -  then/success/error method

```javascript
$http({
		method: 'GET',
		url: '/api/users.json'
	})
	.success(function(data, status, headers, config) {
	
	})
```

### data (string/object)

```javascript
{
	id: 12,
	name: "hemant",
	loc: "Hyderabad"
}
```

### status (number) - HTTP status code of the response

### headers (function) 

```javascript
$http({
	method: 'GET',
	url: '/api/users.json'
}).then(resp) {
	// Fetch the X-Auth-ID
	resp.headers('X-Auth-ID');
}
```

### config (object)

This object is the full, generated configuration object that was used to generate the original request.

## Caching $http response

```javascript
$http.get('/api/users.json', { cache: true })
	.success(function(data) {})
	.error(function(data) {});
```

Take more control over cache

> the latest 20 unique requests will be cached.

```javascript
var lru = $cacheFactory('lru', {
	capacity: 20
});

$http.get('/api/users.json', { cache: lru })
	.success(function(data) {})
	.error(function(data) {});
```

## Enable caching for all http request

```javascript
angular.module('myApp')
	.config(function($httpProvider) {
		$httpProvider.defaults.cache = true;
	});

Or - for custom caching

angular.module('myApp')
	.config(function($httpProvider, $cacheFactory) {
		$httpProvider.defaults.cache =
			$cacheFactory('lru', {
			capacity: 20
		});
	});
```

## Interceptors

Anytime that we want to provide global functionality on all of our requests, such as authentication, error handling, etc., it’s useful to be able to provide the ability to intercep all requests before they pass to the server and back from the server.

> basically it's middleware for the $http service

There are four types of interceptors – two success interceptors and two rejection interceptors

```
request
response
requestError
responseError
```
### Create Interceptors

```javascript
angular.module("myApp")
	.factory('TokenInterceptor', function ($q) {
	    return {
	        request: function (config) {
				
	        },

	        requestError: function(rejection) {
	            
	        },

	        response: function (response) {
		
	        },

	        responseError: function(rejection) {

	        }
	    };
    };
});
```

### Using Interceptors

```javascript
angular.module('myApp')
	.config(function($httpProvider) {
		$httpProvider.interceptors.push('TokenInterceptor');
	});
```

### Update request/response header for every request

```javascript
angular.module('myApp')
	.config(function($httpProvider) {
		$httpProvider.defaults.headers.common['X-Requested-By'] = 'Hemant';
	});
```

## $resource Service

```javascript
angular.module('myApp')
	.factory("restService", function($resource) {
		return $resource("/users/:userId", { userId: "@id"}, { update: { method: "put" }});
	});
```	

$resource takes 3 arguments

1st - url = `/api/users/:id`
2nd - paramDefaults (optional object) = `{id: '123', name: 'Hemant' }`
The second parameter contains the default values for the URL parameters that will be sent with each request.

```javascript
$resource("/users/:id", {id: '123', name: 'Hemant' })
// url becomes  /users/123?name=Hemant
```

To set a dynamic parameter, we only need to prefix the value with a '@' character.

```javascript
angular.module("myApp")
	.factory("User", function($resource){
		return $resource("/users/:id", {id: '@id', name: 'Hemant' })
	});

// controller
angular.module("myApp")
	.controller("MainCtrl", function($scope, User){
		$scope.user = User.get({id: 456});
		// url becomes  /users/456?name=Hemant
	});
```

3rd argument - actions (optional object) = `{ update: { method: "put" }}`
it can have any $http configuration options.

## $resource methods

$resource service  generates 5 methods that allow us to interact with a collection of resources

2 GET method - get, query
3 NON GET method - post, remove, delete	

> GET method accepts 3 arguments  
	- get(params, successFn, errorFn)
	params (object)
	successFn (function)
	errorFn (function)

> HTTP Non-GET Methods can have 4 arguments 
	- save(params, data, successFn, errorFn)
	params (object)
	data {name: "hemant", loc: "Hyderabad"}
	successFn (function)
	errorFn (function)	

### get(params, successFn, errorFn)

get() request is generally used to get a single resource.	

```javascript
// service
angular.module("myApp")
	.factory('User', function($resource){
        return $resource("users", {});
    });

// controller 
- GET /users/123
User.get({
	id: '123'
}, function(resp) {
	// Handle successful response here
}, function(err) {
	// Handle error here
});
```

### query(params, successFn, errorFn)

expects a collection of resource objects as a JSON response array.

```javascript
// GET /users
User.query( function(resp) {
		// Handle successful response here
	}, function(err) {
		// Handle error here
	});
```

### save(params, data, successFn, errorFn)

The save() method is used to create a new resource on the server

```javascript
// POST /users
User.save({
	name: 'Hemant',
	loc: "Hyderabad"
}, function(response) {
	// Handle a successful response
}, function(response) {
	// Handle a non-successful response
});
```

### delete(params, data, successFn, errorFn)

```javascript
// DELETE /users
User.delete({
		id: '123'
	}, function(response) {
		// Handle a successful delete response
	}, function(response) {
		// Handle a non-successful delete response
});
```
### remove(params, data, successFn, errorFn) 

same as `delete`, Use it over delete 

## $resource instances

```
$save
$remove
$delete
```

> called on a single resource instead of on a collection.

```javascript
// Using the $save() instance methods
User.get({id: '123'}, function(user) {
	user.name = 'Hemant';
	user.$save(); // Save the user
});

// This is equivalent to the above
User.save({id: '123'}, {name: 'Hemant'});
```

### Async $resource 

```javascript
// $scope.user will be empty, update automatically with data 
$scope.user = User.get({id: '123'});


User.get({id: '123'}, function(user) {
	$scope.user = user;
});
```

## Custom $resource method

```javascript
// service
angular.module("myApp")
	.factory('User', function($resource){
        return $resource("users", {}, { update: { method: "PUT" }});
    });
```    

Now $resource can be used for making PUT request

```javascript
// PUT /users/123
User.update({id: '123', name: "Papa", loc: "Delhi" }, function(user) {
	$scope.user = user;
});
```

## $resource Configuration Object

The $resource configuration object is very similar to the $http configuration object (with a few
changes).

```
isArray - specific to $resource
```

```javascript
// service
angular.module("myApp")
	.factory('User', function($resource){
        return $resource("users", {}, { update: { method: "PUT", isArray: false,  }});
    });
```    

## Cross Origin Requests

```
JSONP
CORS
Server Proxies - more backend setup
```

### jsonp

- In order to work with JSONP, the server must be able to support the method.
- We can only use JSONP to send GET requests

```javascript
$http
.jsonp("https://api.github.com?callback=JSON_CALLBACK")
.success(function(data) {
	// Data
});
```

### CORS - Cross Origin Resource Sharing

- W3C has created the CORS specification to replace the JSONP hack in a standard way.
- The CORS specification is simply an extension to the standard XMLHttpRequest object that allows
JavaScript to make cross-domain XHR calls.

## Working with XML

```html
<script src="https://x2js.googlecode.com/hg/xml2json.js"></script>
```
```javascript
// services
angular.module("myApp")
	.factory('xmlParser', function() {
		var x2js = new X2JS();
		return {
			xml2json: x2js.xml2json,
			json2xml: x2js.json2xml_str
		};
	});

angular.module("myApp")
	.factory('dataService', function($http, xmlParser) {
		retunr $http.get('/api/msgs.xml', {
			transformResponse: function(data) {
				return xmlParser.xml2json(data);
			}
		});
	});

// controller
angular.module("myApp")
	.controller('MainCtrl', function($scope, dataService) {
		dataService.success(function(res){
			$scope.messages = res;
		});
	});
```



## The Services That Expose DOM API Features

### Global Object Dervices

1. **$anchorScroll** - Scrolls the browser window to a specified anchor
2. **$document** - Provides a jqLite object that contains the DOM window.document object
3. **$interval** - Provides an enhanced wrapper around the window.setInterval function
4. **$location** - Provides access to the URL
5. **$log** - Provides a wrapper around the console object
6. **$timeout** - Provides an enhanced wrapper around the window.setITimeout function
7. **$window** - Provides a reference to the DOM window object