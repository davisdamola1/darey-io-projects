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

* I created files for private and public keys and used the vi editor to add the private key and public key details to them:
```
touch id_rsa id_rsa.pub
```
```
vi id_rsa
```
```
vi id_rsa.pub
```
* I created a group called *developers* which the users would be added to automatically when the script is exexcuted:
```
sudo groupadd developers
```
![Shell - Step 5 - Created developers group](https://user-images.githubusercontent.com/116941965/211261670-7cec243a-dc29-43c1-945e-5687cf3dad84.PNG)
![Shell - Step 9 - Developers group id](https://user-images.githubusercontent.com/116941965/211262916-9c7e4ee5-85d3-43fc-a74c-4b0cb4bf0881.PNG)

* I modified permissions to make the script **onboard.sh** executable, then I executed it superuser mode, which was successful:
```
sudo chmod +x onboard.sh
```
```
sudo su
```
```
./onboard.sh
```
![Shell - Step 6 - Make the onboard sh script executable](https://user-images.githubusercontent.com/116941965/211262264-8c8b3a43-3a37-45e6-a1ba-9d9cbd29e979.PNG)
![Shell - Step 7 - Had to swtich to superuser to execute script, only admin can](https://user-images.githubusercontent.com/116941965/211262310-fd3dd72d-7986-4d5f-bc6e-31d0e2f88e11.PNG)
![Shell - Step 8 - Onboard sh successfully executed](https://user-images.githubusercontent.com/116941965/211262376-5c326dd1-7329-49d3-9fdb-01823ef6aa4e.PNG)

* I confirmed to see if the users were created successfully:
```
ls -l /home/
```
![Shell - Step 8b - proof all the users have been created using ls -l](https://user-images.githubusercontent.com/116941965/211262684-5604c9b8-77e0-44c4-b9b0-d35c72a93124.PNG)

* I also confirmed to see if the users were added to the *developers* group (id 1001) successfully checking in /etc/passwd which is where all users and passwords are stored. Also used *awk* command to filter for this information:
```
cat /etc/passwd
```
```
cat /etc/passwd | awk -F':' '{ print $1}' | xargs -n1 groups
```
![Shell - Step 10 - Users have been added to developers group](https://user-images.githubusercontent.com/116941965/211263173-b6e05010-0df1-4aac-a601-3b7eebcdee0b.PNG)
![Shell - Step 10b - Users have been added to developers group](https://user-images.githubusercontent.com/116941965/211263645-486d2424-b745-4343-b100-689f49cc2f25.PNG)
![Shell - Step 10c - Users have been added to developers group](https://user-images.githubusercontent.com/116941965/211264352-297d6b7b-b7b5-4b68-9dbe-c81847da983f.PNG)
![Shell - Step 10d - Users have been added to developers group](https://user-images.githubusercontent.com/116941965/211264413-db277b2e-fd39-4ab7-bcd2-fefb8f8aae0b.PNG)

* Next step was test that the onboarding was successfully by attempting to connect to a few users. I tested this on 2 users named **Ana** and **Bana**.
* In order for me to connect remotely to these users, I had to create a private key file locally called *aux-proj.pem* and add the private key details that was added to the server for these users. I made sure the permissions for the private key file was updated to prevent others from accessing it using *Chmod 600*.
* I then attempted to connect to the 2 users and it was successful:
``` 
scp -i aux-proj.pem Ana@107.21.86.210
```
```
scp -i aux-proj.pem Bana@107.21.86.210
```
![Shell - Step 11b - Test - Successfully connected to user on server](https://user-images.githubusercontent.com/116941965/211266537-f7070bca-491a-4795-abd9-e11b2617d464.PNG)
![Shell - Step 11c - Test - Successfully connected to user on server](https://user-images.githubusercontent.com/116941965/211266329-dec026da-4304-46a0-9aa2-dcfe83987ba2.PNG)
![Shell - Step 11d - Test - Successfully connected to user on server](https://user-images.githubusercontent.com/116941965/211266386-7e79abfb-f673-42d7-abab-e807e81d6e92.PNG)
![Shell - Step 11e - Test - Successfully connected to user on server](https://user-images.githubusercontent.com/116941965/211266708-e453666f-f46c-4d62-8399-ff3263f7a500.PNG)



