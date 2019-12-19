144

15

There have been some middleware changes on the new version of express and I have made some changes in my code around some of the other posts on this issue but I can't get anything to stick.

We had it working before hand but I can't remember what the change was.

```
throw new TypeError('Router.use() requires middleware function but got a
        ^
TypeError: Router.use() requires middleware function but got a Object
```

```
node ./bin/www

js-bson: Failed to load c++ bson extension, using pure JS version
js-bson: Failed to load c++ bson extension, using pure JS version
```

```
/Users/datis/Documents/bb-dashboard/node_modules/express/lib/router/index.js:438
      throw new TypeError('Router.use() requires middleware function but got a
            ^
TypeError: Router.use() requires middleware function but got a Object
    at /Users/datis/Documents/bb-dashboard/node_modules/express/lib/router/index.js:438:13
    at Array.forEach (native)
    at Function.use (/Users/datis/Documents/bb-dashboard/node_modules/express/lib/router/index.js:436:13)
    at /Users/datis/Documents/bb-dashboard/node_modules/express/lib/application.js:188:21
    at Array.forEach (native)
    at Function.use (/Users/datis/Documents/bb-dashboard/node_modules/express/lib/application.js:185:7)
    at Object.<anonymous> (/Users/datis/Documents/bb-dashboard/app.js:46:5)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
```

app.js

```
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var mongoose = require('mongoose');
var session = require('express-session');
var MongoClient = require('mongodb').MongoClient;
var routes = require('./routes/index');
var users = require('./routes/users');

var Users = require('./models/user');
var Items = require('./models/item');
var Store = require('./models/store');
var StoreItem = require('./models/storeitem');

var app = express();
//set mongo db connection
var db = mongoose.connection; 

MongoClient.connect("mongodb://localhost:27017/test", function(err, db) {
  if(!err) {
    console.log("We are connected");
  }
});
// var MONGOHQ_URL="mongodb://localhost:27017/test" 

// view engine setup
app.set('views', path.join(__dirname, 'views'));

app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(__dirname + '/public/favicon.ico'));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(session({
    secret: 'something',
    resave: true,
    saveUninitialized: true
}));

app.use('/', routes);
app.use('/users', users);
app.use(express.static(path.join(__dirname, 'public')));

// catch 404 and forward to error handler
// app.use(function(req, res, next) {
//     var err = new Error('Not Found');
//     err.status = 404;
//     next(err);
// });

// Make our db accessible to our router
app.use(function(req, res, next){
  req.db = db;
  next();
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: err
        });
    });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
        message: err.message,
        error: {}
    });
});


module.exports = app;
```

It appears the answer to this question has changed for versioning reasons. Thanks to [Nik](https://github.com/expressjs/express/wiki/Migrating-from-3.x-to-4.x)