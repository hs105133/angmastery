# Angular Animation

- Draw Attention
- Convey Idea
- Reduce Complexity
- UX

## Angular Drectives that supports animation

```
ngClass
ngInclude/ngVIew/ngIf/ngSwitch
ngRepeat
ngShow/ngHide
form/ngModel
ngMessages
```

### ngClass

```html
<div ng-class='{"myClass": isOpened}'></div>
```


```css
// add class
.myClass-add{
	transition: color 1s linear;
}

.myClass, .myClass-add-active{
	color: blue;
}

// remove class
.myClass-remove{
	transition: color 1s linear;
}

.myClass-remove-active{
	color: blue;
}
```

![anim props](anim-prop.png)

opacity, translate, scale are more performent than others

```css
even translate3d more performent than translate

// ok
transform: translateX(150px);

// better
transform: translate3d(150px,0,0); // hardware accelareted
```

## ngInclude/ngVIew/ngIf/ngSwitch

```
.ng-enter{
	opacity: 0;
	transition: opacity 1s;
}

.ng-enter-active{
	opacity: 1;
}

// sample
ng-include.ng-enter,[ng-include].ng-enter{
	opacity: 0;
	transition: opacity 1s;
}

ng-include.ng-enter-active, [ng-include].ng-enter-active{
	opacity: 1;
}
```




















