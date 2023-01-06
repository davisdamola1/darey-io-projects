### MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

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

* Within the 'apps' folder, I created a folder named 'models'. Within the 'models' folder, I added a file name book.js and added code:
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
* 





