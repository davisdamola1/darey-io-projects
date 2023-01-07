### MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

The task was to implement a simple Book Register web form using MEAN stack

#### Installing Nodejs:
* Before commencing, I ensured ubuntu was up to date. So I updated and upgraded ubuntu:
```
sudo apt update
sudo apt upgrade
```
* I added certificates as a prerequisite for installing node using:
```
 sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
 curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
 ```
 ![MEAN - Step 1 - Add Certificates](https://user-images.githubusercontent.com/116941965/211064623-73bd5694-4277-483e-b779-b565060b78d4.PNG)
 
 * Then I installed NodeJS using:
```
sudo apt install -y nodejs
```
![MEAN - Step 2 - Install Nodejs](https://user-images.githubusercontent.com/116941965/211075270-e7bae615-cfe9-4327-b8d0-ee79a4a25a6e.PNG)

#### Installing MongoDB
* Before installing MongoDB, I had to import the public key for MongoDB:
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
* I also had to add MongoDB’s APT repository to the /etc/apt/sources.list.d directory:
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
* I proceeded with installing MongoDB:
```
sudo apt install -y mongodb
```
![MEAN - Step 3 - Mongo DB Installation](https://user-images.githubusercontent.com/116941965/211077782-e3c3ffd1-f5b2-4736-9346-d96af8d84ea1.PNG)
* Once installation is complete, I start the server and verify the server is running:
```
sudo service mongodb start
```
```
sudo systemctl status mongodb
```
![MEAN - Step 4 mongodb server started and verified](https://user-images.githubusercontent.com/116941965/211078423-fa53a318-ad43-42a4-9cda-e34108f9da98.PNG)
* Next step is to install npm (Node Package Manager):
```
sudo apt install -y npm
```
![MEAN - Step 5 - Node package manager installed](https://user-images.githubusercontent.com/116941965/211079124-610d4225-7719-41d6-8c55-d5cd4990a042.PNG)
* Installed body-parser:
```
sudo npm install body-parser
```
The package seemed to install, however, I got these error which I wasn't sure of, but found out the cause in later steps:
![MEAN - Step 6 body parser installed with npm](https://user-images.githubusercontent.com/116941965/211081056-269055ee-86eb-4c08-859d-2acd922c8da9.PNG)
* I created a 'Books' folder and initialised the npm project within the 'Books' directory:
```
mkdir Books && cd Books
```
![MEAN - Step 7 - Books directory created and npm project initialised](https://user-images.githubusercontent.com/116941965/211081787-42000c8d-0c42-4478-8eab-e0528cf11e6f.PNG)
* Within the 'Books' directory, I create a file server.js and added the web server code:
```
vi server.js
```
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
![MEAN - Step 8 - config file Server js added using vi server js and web server code added](https://user-images.githubusercontent.com/116941965/211082599-27a5bce7-60b2-458d-9aa9-40544d2ea23e.PNG)
#### Installing Express and Setting up routes to the server
* I installed Express, which will be used to pass book information to and from our MongoDB database. Also installed Mongoose package which we will use to establish a schema for the database to store data of our book register:
```
sudo npm install express mongoose
```
At this point, even though I had gotten errors, I thought it had installed:
![MEAN - Step 9 - Install express and Mongoose](https://user-images.githubusercontent.com/116941965/211083885-edefdcaa-8289-4b91-8547-162e8dbf2301.PNG)

I found out reason in later steps (Keep reading!).
* Within the 'Books' directory, I created 'apps' folder. Within the 'apps' folder, I added a file named route.js and added code:
```
mkdir apps && cd apps
```
```
vi routes.js
```
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
In the ‘apps’ folder, create a folder named models

mkdir models && cd models
Create a file named book.js

vi book.js
Copy and paste the code below into ‘book.js’

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
![MEAN - Step 10 - apps directory created and routes js file created in apps](https://user-images.githubusercontent.com/116941965/211085215-5cbbb6e3-5bb2-4c0d-9291-febc241dd734.PNG)
![MEAN - Step 11 - code added to routes js file](https://user-images.githubusercontent.com/116941965/211085296-ea9b1610-1c33-41cf-8be3-14a20e45553a.PNG)

* Within the 'apps' folder, I created a folder named 'models'. Within the 'models' folder, I added a file named book.js and added code:
```
mkdir models && cd models
```
```
vi book.js
```
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




#### Accessing the routes with AngularJS
* Within the *Books* folder, I created a folder named *public*, then added a file named *script.js* which code was added to:
```
mkdir public && cd public
```
```
vi script.js
```
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
![MEAN - Step 14 - public folder created in books directory and script js file added](https://user-images.githubusercontent.com/116941965/211088007-92f6237b-9fcd-40cb-8484-b12787294675.PNG)
![MEAN - Step 15 - code added to script js file](https://user-images.githubusercontent.com/116941965/211088079-03666b88-0290-4cac-9b07-cae9ff07d078.PNG)

* Within the *public* folder, I then added a index.html file and added code:
```
vi index.html
```
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
![MEAN - Step 16 - index html file created in public folder and code added](https://user-images.githubusercontent.com/116941965/211088384-4f19f79b-2ccd-43a4-958b-16dc4215c031.PNG)

* Next step was to start node server:
```
node server.js
```
This is when I realised there was something wrong, when I got this error whilst trying to start the server:

![MEAN - Step 17 - Error when starting node server](https://user-images.githubusercontent.com/116941965/211088947-08f2d458-7457-43dd-aea1-7b811f2319ea.PNG)

After troubleshooting, I found out this occurs in node 10 or version only.

* To start with, I checked the current version of node installed:
```
node --version
```
The version was v10.19.0, so needed updating.

* I then used npm to install a node version manager called **n**:
```
sudo npm install -g n
```
* Then I proceeded to install stable version of node:
```
sudo n stable
```
![MEAN - Step 18a - Had to install latest version of nodejs](https://user-images.githubusercontent.com/116941965/211150377-644325b9-4fde-49c9-a242-4300e7660249.PNG)

* Now version of node is now *v18.13.0*:

![MEAN - Step 18b - New nodejs version](https://user-images.githubusercontent.com/116941965/211090846-a3428f5e-6c38-4be0-85ff-7be448fa2f47.PNG)

* I run attempt to start node server again and I get an error implying that Express and Mongoose module could not be found:

![MEAN - Step 18c - I run node server js again, and get this error](https://user-images.githubusercontent.com/116941965/211091248-892148ab-db17-4536-a646-09bfaf93cc18.PNG)

It now makes sense why I was getting error when I initially tried to install Mongoose and Express. This could have been because of the version of Node I was on.

* I then re-attempt the installation of Express and Mongoose and seems ok:

![MEAN - Step 18e - Re-installed Express](https://user-images.githubusercontent.com/116941965/211091713-7d7271fa-bdc4-4e18-8846-be82d8e04844.PNG)
![MEAN - Step 18f - Reinstalled Mongoose](https://user-images.githubusercontent.com/116941965/211091797-ced69513-06bb-488a-862e-3ac8c4759e12.PNG)

* Then I re-attempt to start the node server and all seems ok:

![MEAN - Step 18g - nodejs server up now](https://user-images.githubusercontent.com/116941965/211091922-cc2f42a0-cd6a-4185-aba6-aeea8aea361b.PNG)

* The server is now up and running, I attempt to connect it via port 3300.

```
curl -s http://localhost:3300
```

![MEAN - Step 19 - Running curl locally brings back this html page but not readable](https://user-images.githubusercontent.com/116941965/211092066-b78d5b3b-a374-4b53-b391-c62869587a50.PNG)

* Opened in web browser with public ipv4:


![MEAN - Step 19b - Application now running in web](https://user-images.githubusercontent.com/116941965/211092151-992cc499-53be-4d38-a270-e955e674178c.PNG)

