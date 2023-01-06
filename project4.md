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
The package seemed to install, however, I got these error which I wasn't sure of:
![MEAN - Step 6 body parser installed with npm](https://user-images.githubusercontent.com/116941965/211081056-269055ee-86eb-4c08-859d-2acd922c8da9.PNG)
* I created a 'Books' folder and initialised the npm project within the 'Books' directory:
```
mkdir Books && cd Books
```
![MEAN - Step 7 - Books directory created and npm project initialised](https://user-images.githubusercontent.com/116941965/211081787-42000c8d-0c42-4478-8eab-e0528cf11e6f.PNG)







