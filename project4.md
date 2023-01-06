### MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

#### Installing Nodejs:
* I started by adding certificates as a prerequisite for installing node using:
```
 sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
 curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
 ```
 ![MEAN - Step 1 - Add Certificates](https://user-images.githubusercontent.com/116941965/211064623-73bd5694-4277-483e-b779-b565060b78d4.PNG)
 
 * Then I installed NodeJS using:
```
sudo apt install -y nodejs
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
