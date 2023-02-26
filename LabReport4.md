# Lab Report 4

Name: Harsh Gurnani

PID: A17342880

Email: hgurnani@ucsd.edu

Sources used: CSE 15L Course Website - Week 7 

In this lab report, I will be reproducing all the tasks from the class competition that all students participated in during the Week 7 lab section. For each of steps 4 to 9, I will include screenshots, a lot of the keys I pressed, and a summary of the commands I ran.

*Note: I used a fresh terminal window for this task, with no history*

---

## Step 4

*Task: Log into ieng6*

To log into ieng6, we must use the `ssh` command, which is used to log into a remote server. Since the terminal had no history, I could not use the `<up>` key to retrieve a previous command. 

Students must `ssh` into their respective ieng6 accounts. In my case, I ran the command `ssh cs15lwi23apq@ieng6.ucsd.edu <enter>`, with cs15lwi23apq being my CSE 15L course specific account. Since we set up an SSH key for ieng6 in lab, I did not have to enter my password.  

<img width="647" alt="Screenshot 2023-02-25 at 5 13 15 PM" src="https://user-images.githubusercontent.com/68934498/221386978-1d8d119c-14ef-4fb9-8d72-c8cc052d363e.png">

> Output of logging into the ieng6 account. 


## Step 5

*Task: Clone your fork of the repository from your Github account*

Prior to the timed portion of the competition, I created a new fork of the lab 7 repository and titled it 'Lab7New'. To clone the repository from Github, we use the `git clone` command. During lab, we set up SSH keys in ieng6 to connect to Github, meaning we can now use the SSH URL method of cloning repositories. 

<img width="397" alt="Screenshot 2023-02-25 at 5 27 38 PM" src="https://user-images.githubusercontent.com/68934498/221387340-c118918e-f24c-41f8-9dbe-1184512b1536.png">

> Obtaining the SSH URL for cloning looks like this. Click the copy button to copy it to your clipboard.

In my terminal, I used the command `git clone git@github.com:ucsd-cse15l-w23/lab7.git <enter>` to clone the Lab7New repository.

<img width="647" alt="Screenshot 2023-02-25 at 5 30 37 PM" src="https://user-images.githubusercontent.com/68934498/221387415-d95262ba-7839-4ddb-8005-0c131430944d.png">

> Cloning the repo on terminal


## Step 6

*Task: Run the tests, demonstrating that they fail*

After the previous step, I needed to enter the recently cloned directory. To do so, I used `ls <enter>' to see the available directories, and then 
