# Node JS Basics
install node from http://www.nodejs.org/

- node
- npm

## Create Express App

```
npm install express --save
express myApp
```

## server.js

```javascript
var express = require("express"),
	app = express();

app.set("view engine", "jade");
app.set("views", __dirname);
app.use(express.static(__dirname+'/public'));

app.get("*", function(req, res){
	res.render("index");
});	

app.listen(porocess.env.PORT, process.env.IP); // c9

```



