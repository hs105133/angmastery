# Angular JS Filters

Filter provides a way to format the data we display to the user.

## Applying Filter

in HTML `{{ name | uppercase }}`

or 

from within javascript...

```javascript
angular.module("myApp")
  .controller('DemoController', function($scope, $filter) {
    $scope.name = $filter('uppercase')('Hemant'); // HEMANT
  });
```

## Filter with argument

```html
<!-- Displays: 123.46 -->
{{ 123.456789 | number:2 }}
```

## 1. Filters that operates on single values

### currency - This filter formats currency values.

```html
<!-- 1.2 -> $1.20 -->
<p>Price: {{ product.price | currency }}</p>
```
### date - This filter formats date values.

```html
// 1409995767615 ->  Sep 6, 2014
<p>Expiry: {{ product.expiry | date }}</p>
```

### json - This filter generates an object from a JSON string.

```html
// { name: "hemant"} -> { "name": "hemant"}
<p>{{ user }}</p> 
```

### number - This filter formats a numeric value.

```html
// 7799111008 -> 7,799,111,008
<p>your Balance: {{ user.balance |  number }}</p>
```
### uppercase - Format string to uppercase

```html
// hemant -> HEMANT
<p>Name: {{ user.name | uppercase }}</p>
```
### lowercase - Format string to uppercase

```html
// Hemant -> hemant
<p>Category: {{ user.name | lowercase }}</p>
```

## 2. Filters that operates on collection

### limitTo - Limiting the Number of Items

```html
<tr ng-repeat="p in products | limitTo:limitVal">  </tr>
```

### filter - Filtering collection based on some key ( string, object, function)

- key as string

```html
<p>{{ ['Hemant', 'Singh', 'Varun', 'Vinay', 'Ajay'] | filter:'n' }}</p>
<!-- ["Hemant","Singh","Varun","Vinay"] -->
```

- key as Object

```html
<p>  
  [{
      'name': 'Varun',
      'City': 'Hyderabad'
    }, 
    {
      'name': 'Hemant',
      'City': 'Blr'
  }] | filter: { 'name': 'Hemant'}
</p>
<!-- [{"name":"Hemant","City":"Blr"}] -->
```

- key as function

```html
{{ ['Hemant', 'likes', 'to', 'travel'] | filter:isCapitalized }}
<!-- ["Hemant"] -->
```

```javascript
$scope.isCapitalized = function(str) { 
  return str[0] == str[0].toUpperCase(); 
}
```

### orderBy - Sorting items (asc/desc)

```html
// Ascending
<tr ng-repeat="p in products | orderBy:'price'">  </tr>

// Descending
<tr ng-repeat="p in products | orderBy:'-price'">  </tr>
```

## Chaining Filters

```html
<tr ng-repeat="p in products | orderBy:[myCustomSorter, '-price'] | limitTo: 5">  </tr>
```

## Creating Custom Filters

Create a filter that display `New` if item created in last 2 days 

Demo - http://embed.plnkr.co/Sv3QpzvGWqoUTqQgoyGZ/preview

```javascript
angular.module("myApp").filter("showNewLabel", function(){
  return function(input, option){
    var inputDate = new Date(input).getDate(),
        currentDate = new Date().getDate();
    
    return currentDate - inputDate >= 2 ? true : false; 
 };
});
```

```html
<span ng-show="product.expiry | showNewLabel">New</span>
```

## Creating Collection Filter

Create a filter that skip some user in users collecion

Demo - http://embed.plnkr.co/fNeCAfsz0kwlm4ujfF9n/preview

```javascript
angular.module("myApp").filter("skip", function(){
  return function(input, option){
    return option ? input.slice(option) : input;
  }
})
```

```html
<li ng-repeat="user in users | skip:2">
	<p>{{ user.name | uppercase }}</p>
</li>
```

## Reuse existing Filter

Create a filter that skip some user anf take only few users in users collecion

Demo - http://embed.plnkr.co/Ynmv0zePGy2RIo6FMk4Y/preview

```javascript
angular.module("myApp").filter("skip", function(){
  return function(input, option){
    return option ? input.slice(option) : input;
  }
}).filter("take", function($filter){
  return function(input, skipCount, takeCount){
    var skippedData = $filter("skip")(input, skipCount);
    return $filter("limitTo")(skippedData, takeCount);
  };
});
```

```html
<li ng-repeat="user in users | take:2:5">
	<p>{{ user.name | uppercase }}</p>
</li>
```

