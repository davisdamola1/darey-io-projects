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


##### Body-parser package was installed with node package manager but I get this message:
*npm WARN saveError ENOENT: no such file or directory, open '/home/ubuntu/package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open '/home/ubuntu/package.json'
npm WARN ubuntu No description
npm WARN ubuntu No repository field.
npm WARN ubuntu No README data
npm WARN ubuntu No license field*
##### Seems to have installed however.
