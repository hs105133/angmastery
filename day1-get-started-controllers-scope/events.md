# Angular Event

## Bubbling an Event Up with $emit

```javascript
scope.$emit('user:logged_in', scope.user);
```

> If we want to communicate with our $rootScope then we need to $emit() the event.

## Sending an Event Down with $broadcast

To pass an event downwards (from parent scopes to child scopes), we use the $broadcast() function

```javascript
scope.$broadcast('cart:checking_out', scope.cart);
```

## Listening Events

We can listen event using $on method

```javascript
scope.$on('$routeChangeStart', function(evt, next, current) {
	// A new route has been triggered
});
```

## Event Object attributes

### targetScope (scope object) 
This attribute is the scope emitting or broadcasting the event.

### currentScope (scope object)
This object contains the current scope that is handling the event.

### name (string)
This string is the name of the event that was fired and that we are handling.

### Others

```
stopPropagation (function)
preventDefault (function)
defaultPrevented (boolean) - want to cancel preventDefault set this to false 
```

## Event emitted from scope

```javascript
$scope.$on('$includeContentLoaded', function(evt) {

});
```

### $includeContentLoaded 

fires from the ngInclude directive when the ngInclude content is reloaded

### $includeContentRequested

Fires on the scope when ngInclude content is requested.

### $viewContentLoaded
The $viewContentLoaded event is emitted on the current ngView scope every single time that ngView
content is reloaded.

## Core System $broadcasted Events

- $locationChangeStart
- $locationChangeSuccess
- $routeChangeStart
- $routeChangeSuccess 
broadcasted from the $rootScope after all of the route dependencies are resolved

- $routeChangeError
The $routeChangeError event is fired if any of the resolve properties on the route object are rejected

- $routeUpdate
The $routeUpdate is broadcasted from the $rootScope if the reloadOnSearch property on the $routeProvider has been set to false and the same instance of a controller is being used.
- $destroy












