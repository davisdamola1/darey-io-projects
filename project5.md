##  Project 5 - Aux Project 1 - Shell Scripting
###  The task was to onboard 20 new Linux users onto a server using a shell script that reads a csv file that contains the first name of the users to be onboarded.

* My first step was to create a file containing the 20 names called **names.csv** using *vi names.csv* command locally:

![Shell - Step 1b- Added 20 random names to the names csv file](https://user-images.githubusercontent.com/116941965/211257958-8a19a567-3133-4929-a10c-73ed9b26a5f8.PNG)

* I created my shell script **onboard.sh** which will be used to execute the onboarding of the 20 users:

![Shell - Step 1c - created shell script for onboard](https://user-images.githubusercontent.com/116941965/211258632-74a69fad-334b-4508-89fe-a293076072bc.PNG)

* Once both files were created, I transfer them to the AWS server I would be onboarding the users on:
```
scp -i Damola2.pem onboard.sh ubuntu@107.21.86.210:~/;
```
```
scp -i Damola2.pem names.csv ubuntu@107.21.86.210:~/;
```
![Shell - Step 1d - Sent onboard script to AWS instance](https://user-images.githubusercontent.com/116941965/211259305-9caa56fd-9229-4889-83f7-696c4229517d.PNG)
![Shell - Step 1e  - Move the names csv file to server](https://user-images.githubusercontent.com/116941965/211259394-5949fe4b-6032-491d-8655-a1bbd79e9a7f.PNG)

* I connected to my AWS ubuntu server
* I created a new folder called **Shell** and moved the script and the csv file into it:
```
mkdir Shell
mv names.csv ~/Shell
mv onboard.sh ~/Shell
```
![Shell - Step 2 - Created a new folder called Shell and added names csv file](https://user-images.githubusercontent.com/116941965/211260446-d64192f8-df7b-4812-87da-e09b364a801f.PNG)
![Shell - Step 2b - Also moved the onboard sh into Shell folder](https://user-images.githubusercontent.com/116941965/211260533-041fcab3-972b-41b3-8293-6b5169929145.PNG)

I created files for private and public keys and used the vi editor to add the private key and public key details to them:
```
touch id_rsa id_rsa.pub
```
```
vi id_rsa
```
```
vi id_rsa.pub
```
![Shell - step 3 - created file for private and public key](https://user-images.githubusercontent.com/116941965/211261104-6dc8f22a-88ad-4410-b24e-468bf7c8648b.PNG)
![Shell - Step 4 - Added public key to public key file](https://user-images.githubusercontent.com/116941965/211261152-afd7c4c5-6117-4807-b3ac-d09237ddb195.PNG)
![Shell - Step 4b - Add private key to private key file](https://user-images.githubusercontent.com/116941965/211261205-18ce9eb9-7020-4f14-aeba-3501bfcdd005.PNG)




