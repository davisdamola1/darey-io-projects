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
