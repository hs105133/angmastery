# Angular JS Services in Depth

## 1. The Services That Expose DOM API Features

### Global Object Dervices

1. **$anchorScroll** - Scrolls the browser window to a specified anchor
2. **$document** - Provides a jqLite object that contains the DOM window.document object
3. **$interval** - Provides an enhanced wrapper around the window.setInterval function
4. **$location** - Provides access to the URL
5. **$log** - Provides a wrapper around the console object
6. **$timeout** - Provides an enhanced wrapper around the window.setITimeout function
7. **$window** - Provides a reference to the DOM window object

## $http service in action


```javascript
// Service
angular.module("myApp").factory("BookService", function($http){
  return $http.get("http://hkapi.herokuapp.com/books"); // returns a promise
});

// Controller
angular.module("myApp").controller("BookService", function(UserService){
  UserService.success(function(res){
    $scope.books = res;
  });
});
```

## Making Ajax Request with $http

We can make Ajax request in two ways...

1. using `$http(config)`
2. using `$http.xxx(url, config)`  for get operation and $http.xxx(url, data, config) for non get operations.

`config` is object and it's optional

for ex...
```javascript
{ 
    method: "GET/POST/PUT/DELETE", 
    url: "/books", 
    params: "key1=value1&key2=value2"  or { key1: value1, key2: valu2 }
    data: { id:20, name: "Hemant Singh" },
    headers: 
    cache: true/false
}
```

#### **Configuration Properties for Angular JS $http service**

Name| Description
----|------
url| Set url for server request
data| data you want to send to server


for all available arguments browse https://docs.angularjs.org/api/ng/service/$http




