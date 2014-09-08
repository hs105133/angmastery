# Angular JS Filters

## 1. Filters that operates on single values

### currency - This filter formats currency values.

```html
// 1.2 -> $1.20
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

### filter - Filtering collection based on some key

```html
<tr ng-repeat="p in products | filter:{category: 'Fish'}"> </tr>
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

