# Mongo DB + Angular JS

## c9 Mongo DB setup

> c9 comes with MongoDB pre-installed

- Step 1 - create data folder in root
- Step 2 -  create mongod file ( without any extension) in root

```
// mongod
mongod --bind_ip = $IP --dbpath=data --nojourna1 --rest "$@" 

```

- Step 3

```
chmod 777 mongod 
./mongod
npm install mongoose --save
```

```javascript
// server.js
var mongoose = require("mongoose");

mongoose.connect("mongodb://localhost/books");
var con = mongoose.connection;

con.once("open", function(){
	console.log("connected to mongo db successfully");
});
```

- Step 4 - Create Model (book)

> /models/book.js

```javascript
var mongoose = require("monoose");

var bookSchema = mongoose.schema({
	name: {type:String},
	description: {type:String}
});

var book = mongoose.model("book", bookSchema);

exports.createBook = function(){
	book.find({}).exec(function(err, collection){
		if(collection.length === 0){
			book.create({name: "Java", description: "Java Book"});
			book.create({name: "PHP", description: "PHP Book"});
			book.create({name: "CSS", description: "CSS Book"});
		}
	});	
};

```

> /server.js

```javascript
var bookModel = require("./models/book");

con.once("open", function(){
	console.log("connected to mongo db successfully");
	bookModel.createBook();
});
```

## Verify if book created in DB

Open terminal...

```
mongo
show dbs
use bookModel
show collections
db.book.find()
```
or from browser...

```javascript
app.get("/api/books", function(req, res){
	mongoose.model("book").find({}).exec(function(err, collection){
		res.send(collection);
	});	
});	
```

## connecting to `mongolab` ( production )

- create db
- create user
- copy connection uri ( replace use/pass with your own )

```
// connection URI
mongodb://<dbuser>:<dbpassword>@374573f.mongolab.com:27427/books
```

## Using API on frontend with Angular JS

### Step 1 - inject `ngResource` in module as dependency
```javascript
angular.module("myApp", "ngResource");
```

### Step 2 - Create Service

```javascript
angular.module("myApp")
	.factory("bookSvc", function($resource){
		return $resource("/api/books");
	});
```

### Step 3 - Create Controller that use service

```javascript
angular.module("myApp")
	.controller("BookCtrl", function($scope){
		$scope.books = bookSvc.query();
	});
```




