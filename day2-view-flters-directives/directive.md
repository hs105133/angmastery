## Angular Directive


- Element Directive - `<social-icons></social-icons>`

- Attribute Directive - `<div social-icons></div>`

- Class & Comment directive (don't use it)

```
<div class="my-directive"></div>
<!-- directive: my-directive -->
```

## Create Directive

```javascript
angular.module('myApp')
	.directive("socialIcons", function(){
		return {
			restrict: "EA",
			template: '<a href="http://google.com">Google</a>'
		};
	});
```	

## Directive scope - `creating a new child scope in the DOM`

```html
<p>We can access: {{ rootProperty }}</p>
<div ng-controller="ParentController">
    <p>We can access: {{ rootProperty }} and {{ parentProperty }}</p>
    <div ng-controller="ChildController">
        <p>
            We can access: 
            {{ rootProperty }} 
            and {{ parentProperty }} 
            and {{ childProperty }}
        </p>
        <p>{{ fullSentenceFromChild }}</p>
    </div>
</div>
```

## Passing data to directive

- Using Parent controller scope 

```html
<div ng-controller="TestCtrl">
	<social-icons></social-icons>	
</div>	
```

```javascript
angular.module('myApp', [])
	.controller("TestCtrl", function($scope){
		$scope.linkHref = "http://yahoo.com";
		$scope.linkText = "Yahoo";
	})
	.directive("socialIcons", function(){
		return {
			restrict: "EA",
			template: '<a href="{{ linkHref }}">{{ linkText }}</a>'
		};
	});
```

- Passing data to directive using attributes ( Isolated Scope )

```html
<div ng-controller="TestCtrl">
      <social-icons link-href="http://yahoo.com" link-text="Yahoo">
      </social-icons>
</div>	
```

```javascript
angular.module('myApp', [])
	.directive("socialIcons", function(){
		return {
			restrict: "EA",
			scope:{
				linkHref: "@",
				linkText: "@"
			},
			template: '<a href="{{ linkHref }}">{{ linkText }}</a>'
		};
	});
```

## Angular attribute directive

• ng-href
```html
<!-- Always use ng-href when href includes an {{ expression }} -->
<a ng-href="{{myHref}}">I'm feeling lucky, when I load</a>
```
• ng-src
```html
<!-- Always use ng-src when src includes an {{ expression }} -->
<img ng-src="{{imgSrc}}" />
```

• ng-disabled - boolean
• ng-checked - boolean
• ng-readonly - boolean
• ng-selected - boolean
• ng-class
• ng-style

## Directive that creates child scope

### ng-controller
### ng-include

```html
<div ng-include="'views/header.html'" onload="headerTemplateLoaded()"></div>
```
### ng-switch - 
Used in conjunction with on="propertyName", ng-switch-when & ng-switch-default

```html
<input type="text" ng-model="color" />
<div ng-switch on="person.name"></div>
<h1 ng-switch-when="red">My favorite color is Red</h1>
<h1 ng-switch-when="green">My favorite color is Green</h1>
<p ng-switch-default>My favorite color is Pink</p>
```

### ng-repeat

```
• $index
• $first
• $middle
• $last
• $even
• $odd
```
### ng-view - `<div ng-view></div>`
### ng-if  

Remove DOM if expression evaluate false

### ng-init
### ng-bind
### ng-bind-template 

bind multiple expressions to the view

```html
<div ng-bind-template="{{ message }} {{ name }}"> </div>
```
### ng-cloak - `Avoid flickering `

```html
<body ng-init="greeting = 'Hello World'">
	<p ng-cloak>{{ greeting }}</p>
</body>
```	

```stylesheet
body {
	background: red;
}
```

### ng-form - `Most useful when dynamically generating form fields`

```html
<form name="signup_form" ng-submit="submitForm()" novalidate>
    <div ng-repeat="field in fields" ng-form="signup_form_input">
        <input type="text" name="dynamic_input" ng-required="field.isRequired" ng-model="field.name" placeholder="{{field.placeholder}}" />
        <div ng-show="signup_form_input.dynamic_input.$dirty &&
signup_form_input.dynamic_input.$invalid">  
            <span class="error" ng-show="signup_form_input.dynamic_input.$error.required">
                The field is required.
            </span>
        </div>
    </div>
    <button type="submit" ng-disabled="signup_form.$invalid">
        Submit All
    </button>
</form>
```

```javascript
angular.module('myApp', [])
    .controller("FormCtrl", function($scope) {
        $scope.fields = [{
            placeholder: 'Username',
            isRequired: true
        }, {
            placeholder: 'Password',
            isRequired: true
        }, {
            placeholder: 'Email (optional)',
            isRequired: false
        }];
        $scope.submitForm = function() {
            alert("it works!");
        };
    });
```

### ng-select 

Used in conjunction with ng-options and ng-model

* for array data sources:
	– label for value in array
	– select as label for value in array
	– label group by group for value in array
	– select as label group by group for value in array track by trackexpr

* for object data sources:
	– label for (key, value) in object
	– select as label for (key, value) in object
	– label group by group for (key, value) in object
	– select as label group by group for (key, value) in object

```html
<div class="container" ng-controller="TestCtrl">
    <select ng-model="city" ng-options="city.name for city in cities">
        <option value="">Choose City</option>
    </select>
    Best City: {{ city.name }}
</div>
```    
```javascript
angular.module('myApp', [])
    .controller("TestCtrl", function($scope) {
        $scope.cities = [{
            name: 'Seattle'
        }, {
            name: 'San Francisco'
        }, {
            name: 'Chicago'
        }, {
            name: 'New York'
        }, {
            name: 'Boston'
        }];
    });
```
### ng-class - `Dynamically set class on any element `

```html
<div ng-clas="{'red': x > 5}"></div>
```

### Others

```
ng-model
ng-show
ng-hide
ng-change - `must use in conjunction with ng-model`
ng-click
ng-submit

```

## Custom Directive Options

```html

angular.module('myApp', [])
    .directive('socialIcons', function(){
    	// Runs during compile
    	return {
    		// name: '',
    		// priority: 1,
    		// terminal: true,
    		// scope: {}, // {} = isolate, true = child, false/undefined = no change
    		// controller: function($scope, $element, $attrs, $transclude) {},
    		// require: 'ngModel', // Array = multiple requires, ? = optional, ^ = check parent elements
    		// restrict: 'A', // E = Element, A = Attribute, C = Class, M = Comment
    		// template: '',
    		// templateUrl: '',
    		// replace: true,
    		// transclude: true,
    		// compile: function(tElement, tAttrs, function transclude(function(scope, cloneLinkingFn){ return function linking(scope, elm, attrs){}})),
    		link: function($scope, iElm, iAttrs, controller) {
    			
    		}
    	};
    });
```  

### priority (number)

ngRepeat has the highest priority `1000` of any built-in directive.

```html
<div mydir1 mydir2 ng-repeat=" "></div>
``` 
if mydir1 have priority 1, mydir2 have priority 12

then execution order is... `ng-repeat -> mydir2 -> mydir1`

### scope

By default child directive have access to it's parent directive scope

`scope: false` // default, have access to parent scope

`scope: true` - a new scope object is created that prototypically inherits from its parent
scope.

`scope: {}` - Create Isolate scope ( helpul for creating re-usable widgets )

### Pass String to directive ( Isolate Scope @)
### Two way Binding (=)  
### One way binding (&, parent-child)


## Some Demos

- directive talking to controller
- pass much control to directive (attrs)


























































