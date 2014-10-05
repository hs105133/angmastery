## Angular Module:

* Create Angular module `angular.module("myApp", ["ngRoute"])`

* Use Angular module
 
```
angular.module("myApp").controller(function($scope){

});
```

* Get name of module - `angular.module("myApp").name` // myApp

* Get dependencies of module - `angular.module("myApp").requires`  // ["ngRoute"]

## Angular module Methods

### config

Do something before view shown to user

```javascript
angular.module('myApp', [])
	.config(function($routeProvider){
		
	});
```

### run

Do something before view renders

```javascript
angular.module('myApp', [])
	.run(function($rotScope){
		$rotScope.name = "Hemant";
	});
```


## Force $scope execution 

If the callback executes outside of the Angular context, we can force the $scope to have
knowledge of the change using the $apply method.

```html
<div> {{ clock }}</div>
```

```javascript
angular.module('myApp', [])
    .controller('TestCtrl', function($scope) {
        $scope.clock = new Date();

        setInterval(function() {
            $scope.$apply($scope.clock = new Date());
        }, 1000)
    });
```