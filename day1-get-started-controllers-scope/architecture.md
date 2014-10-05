# Angular Application Architecture

## Recommanded directory structure ( Mediam size app)

```
|-test
|-bower_components
|-images
|-styles
|-views
|-scripts
	|-config
	|-controllers
	|-directives
	|-filters
	|-services
	|-vendor
	|-app.js
```	

### Recommanded `test` directory structure

```
|-config
|-controllers
|	|-homeSpec.js
|-directives
|	|-homeDirectiveSpec.js
|-filters
|-services
|-appSpec.js

```	

## Modularize your app

```
angular.module('slideDirectives', []);
angular.module('slideServices', []);
angular.module('slideFilters', []);
angular.module('slideControllers', ['slideServices']);

angular.module('slideApp', [
	'slideDirectives',
	'slideServices',
	'slideFilters',
	'slideControllers'
]);
```
