# Project4-MEAN-WEB-STACK

### This Project covers the MEAN (MongoDB, Express, Angular and NodeJS) deployment on Ubuntu server.

Some little self learning were done on the following topics; OSI model, Load balancing and did some practice on HTML, CSS and Javascript.

### Step 1
NodeJs was install by first updating `sudo apt update` then, upgrading `sudo apt upgrade` and finally running an installation command `sudo apt install -y nodejs`.

![update](https://user-images.githubusercontent.com/46185705/132374060-e104ee16-cc78-4f97-b646-4b352b77acfc.jpg)
![upgrade](https://user-images.githubusercontent.com/46185705/132374078-67591523-1659-4ee5-98a5-c155d33d2b88.jpg)
![nodejs-installed](https://user-images.githubusercontent.com/46185705/132374111-b6c10952-1375-4862-b6ff-faba89a5f2e3.jpg)

### Step 2 (Mongo DB Installation)

MongoDB is store Data in flexible JSON-like documents. On this project, we added book records to MongoDB that contain info like book name, isbn number, author, and number of pages. And the following command were first ran before the installation command `sudo apt install -y mongodb`;

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`
`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

![mongo](https://user-images.githubusercontent.com/46185705/132377581-aaeb5053-7027-4fc1-8c73-4fb8008fe899.jpg)
![mongodb-installed](https://user-images.githubusercontent.com/46185705/132377593-4d32034b-925f-468b-8b12-b5020ce4eebd.jpg)

Afterwards, the server were started `sudo service mongodb start` and the service were verified `sudo systemctl status mongodb` to know if its up and running

![mongodb-status](https://user-images.githubusercontent.com/46185705/132380458-79c6e091-6c32-4bf2-bbd6-89f352056218.jpg)

Node Package manager was installed `sudo apt install -y npm` followed by a body parser which will help with the JSON file processing `sudo npm install body-parser`. A folder `Book` was created and directory was changed to it using `mkdir Books && cd Books`. NPM was initiated from this `npm init` while a server code was pasted into a file created called __server.js__. Opened with this `vi server.js` and pasted the following 
```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```
![server js](https://user-images.githubusercontent.com/46185705/132500087-fd4fdb95-6885-4c45-a171-e1517e5e1088.jpg)
![books-directory](https://user-images.githubusercontent.com/46185705/132500104-5c2f644d-4b81-4789-ad23-e389ad348c14.jpg)


### Step 3 (ExpressJS installation and route to server)

Mongoose was first installed `sudo npm install express mongoose` as it provides schema-based solution to our application data

![mongoose-installed](https://user-images.githubusercontent.com/46185705/132502660-e68676dd-220c-4aa5-98bb-b21c2057e3c1.jpg)

Inside the Books folder, a directory was created and CDed into it using `mkdir apps && cd apps`. A file was created and opened with a code pasted inside of it
`vi routes.js` 
```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```
![route js](https://user-images.githubusercontent.com/46185705/132503325-ac838086-776c-4c58-84fa-d22294ebda31.jpg)

Another models directory was created and i CDed into it `mkdir models && cd models`. Another file was added and some codes pasted in it as well

`vi book.js`
```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```

### Step 4 (Accessing the route with Angular)

On this stage, Angular was used to connect our webpages with Express and perform actions on our books register.

First of all, the directory was changed back to __Books__ `cd ../..` and a directory called ___Public___ was created `mkdir public && cd public`. A file named script.js was created using `vi script.js` and the following code was ran

```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
```

In the same public folder, `vi index.html` was used to create an index.html file and an html code was pasted into it

```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```

![html file](https://user-images.githubusercontent.com/46185705/132508402-3b2244bd-b720-46e8-8a3e-d27739470081.jpg)

Directory changed back to Books and the node server was started `node server.js`

TCP prot set to 3300

and the final output is show below using the public IP at port 3300 

`18.218.23.139:3300`

![tcp-3300](https://user-images.githubusercontent.com/46185705/132509472-60c5c3a6-69a2-4a14-bf67-3f57f2c3c0c9.jpg)
![Completion](https://user-images.githubusercontent.com/46185705/132509482-fc0d4d3c-22fc-4f61-9832-7f5b96794de9.jpg)

