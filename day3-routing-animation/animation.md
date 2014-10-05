# Angular Animation

## Built-in Directive that support animation

Directive| Names
---------|-----------
ng-repeat| enter, leave, move
ng-view | enter, leave
ng-include | enter, leave
ng-switch | enter, leave
ng-if | enter, leave
ng-class | add, remove
ng-show | add, remove
ng-hide | add, remove

```javascript
angular.module("myApp", ["ngAnimate"]);
```

```html
<div ng-view class="slideAnimation"></div>
```

```stylesheets
.slideAnimation.ng-enter{
	opacity: 0;
	transition: 2s linear all;
}

.slideAnimation.ng-enter-active{
	opacity: 1;
}

```

## Support Touch Events

- Use ngTouch module that provides $swipe service
- The ngTouch swipe events can be used to detect left-to-right and right-to-left swipe gestures
- replacement of ng-click for touch devices

```html
<div class="well"
	ng-swipe-right="handleSwipe('left-to-right')"
	ng-swipe-left="handleSwipe('right-to-left')">
</div>
```

```javascript
angular.module("exampleApp", ["ngTouch"])
	.controller("defaultCtrl", function ($scope, $element) {
		$scope.handleSwipe = function(direction) {
			// handle swipe event here
		}
	});
```
