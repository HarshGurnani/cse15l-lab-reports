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

Prior to the timed portion of the competition, I created a new fork of the lab 7 repository and titled it "Lab7New". To clone the repository from Github, we use the `git clone` command. During lab, we set up SSH keys in ieng6 to connect to Github, meaning we can now use the SSH URL method of cloning repositories. 

<img width="397" alt="Screenshot 2023-02-25 at 5 27 38 PM" src="https://user-images.githubusercontent.com/68934498/221387340-c118918e-f24c-41f8-9dbe-1184512b1536.png">

> Obtaining the SSH URL for cloning looks like this. Click the copy button to copy it to your clipboard.

In my terminal, I used the command `git clone git@github.com:ucsd-cse15l-w23/lab7.git <enter>` to clone the Lab7New repository.

<img width="647" alt="Screenshot 2023-02-25 at 5 30 37 PM" src="https://user-images.githubusercontent.com/68934498/221387415-d95262ba-7839-4ddb-8005-0c131430944d.png">

> Cloning the repo on terminal


## Step 6

*Task: Run the tests, demonstrating that they fail*

After the previous step, I needed to enter the recently cloned directory. To do so, I used `ls <enter>` to see the available directories, and then `cd lab7New <enter>` to actually enter the required directory.

Doing `ls <enter>` once inside, we can see the available files, two of them being `ListExamples.java` and `ListExamplesTests.java`. To run the tests, I first went to [Week 3 CSE 15L website](https://ucsd-cse15l-w23.github.io/week/week3/#setup) and copy pasted the jar commands from the Set Up section (keys pressed were `<command + c>` to copy and `<command + v>` to paste). For each command, I hit `<enter>` to run them. Note that for the command to actually run the tests (not the compilation), I hit `<delete>` multiple times to get rid of the file name, and added `ListExamplesTests` at the end.
  
From the JUnit output, it is obvious that the tests failed. There were 2 tests total, and one of them failed.

<img width="697" alt="Screenshot 2023-02-25 at 5 52 58 PM" src="https://user-images.githubusercontent.com/68934498/221388026-56f15d29-9866-4fd8-8373-688a56b97690.png">

> Running the tests on terminal. We can see from the JUnit output saying "FAILURES!!!" that the tests failed.
  
  
## Step 7

*Task: Edit the code file to fix the failing test*

Once again reading the JUnit output, we see that the error is in `merge` in ListExamples.java. We can use the `nano` command to enter an environment to edit the code. In terminal, I ran the command `nano ListExamples.java <enter>`, resulting in a text editing environment that looked like this: 
  
<img width="699" alt="Screenshot 2023-02-25 at 5 53 55 PM" src="https://user-images.githubusercontent.com/68934498/221388045-0fb73724-a402-4605-a72b-99d520ef0e04.png">

> The nano text editing environment
  
We can hit the `<down>` key multiple times to go to the the merge method. Alternatively, we can also use `<ctrl w>` and search for the text "merge". I chose to do the latter and hit <enter> after typing "merge". 
  
<img width="695" alt="Screenshot 2023-02-25 at 5 55 43 PM" src="https://user-images.githubusercontent.com/68934498/221388083-65ff683f-4f4d-4b7d-9e0b-d99ee4974ef0.png">

> Result after searching for "merge"
  
The error is down in the last while loop - instead of doing `index1 += 1`, we need to do `index2 += 2`. This time, I hit the `<down>` key until I reached the correct line, the `<left>` key 6 times to reach "index1", and the `<delete>` key to get rid of it. Then, I typed in "index2" instead. 
  
<img width="240" alt="Screenshot 2023-02-25 at 5 57 56 PM" src="https://user-images.githubusercontent.com/68934498/221388126-1d71dc02-4119-458d-87c7-a10d42deccc0.png">

> Post editing code

Finally, to save and exit the text file, we use `<ctrl + o>` to save, `<enter>` to save the file name, and `<ctrl + x>` to exit. If we were to reenter the file, we would see our saved changes. Instead of doing so, we can move on to step 8 and see our tests pass, which would demonstrate that our code has been fixed.
  
## Step 8
  
*Task: Run the tests, demonstrating that they now succeed*
  
Now that we have some history in our terminal, we can use the `<up>` button to go to the necessary command. Since I used `ls <enter>` in between to ensure that all the necessary .class files were present, I hit `<up` 4 times to get to the jar command used to compile all the java files, and hit `<enter>`. Then, I hit `<up>` 3 times to reach the second command, and once again hit `<enter>` to run the tests.
  
<img width="696" alt="Screenshot 2023-02-25 at 6 06 01 PM" src="https://user-images.githubusercontent.com/68934498/221388332-939569e9-2fbc-426e-9b66-56c3f9337d88.png">

> JUnit output showing that tests were passed.
  
This time, we see two dots, meaning that the tests passed correctly. Our code now works.
  
## Step 9
  
*Task: Commit and push the resulting change to your Github account (you can pick any commit message!)*
  
The last tasks involved updating the remote version of our code on Github with terminal commands. Firstly, we can use `git status <enter>` to see which files are staged for a commit and which aren't.
  
<img width="532" alt="Screenshot 2023-02-25 at 6 07 29 PM" src="https://user-images.githubusercontent.com/68934498/221388379-c432d2c2-7a44-45b0-b74c-0a6b96c42ec5.png">

> Currently, no files are staged for a commit.

We only want to add the updates to `ListExamples.java` and not the .class files to our remote repository. As such, we can run `git add ListExamples.java <enter>`, and then `git status` again to see that it has been staged. 
  
<img width="494" alt="Screenshot 2023-02-25 at 6 09 08 PM" src="https://user-images.githubusercontent.com/68934498/221388415-8dcd9ce8-5e0d-4b72-99a1-72a042b5e8f0.png">

> ListExamples.java has been staged for a commit.
  
Finally, the last step is just committing our changes and pushing it to Github. To do so, I used `git commit -m "message" <enter>` to commit, and `git push <enter>` to push my changes. The message can be anything you want - I chose to write "Fixed buggy code". 
  
<img width="344" alt="Screenshot 2023-02-25 at 6 13 22 PM" src="https://user-images.githubusercontent.com/68934498/221388537-5b66f837-a5c0-417e-8a34-73cdbc2e70f8.png">

> Result of commit
  
<img width="695" alt="Screenshot 2023-02-25 at 6 14 09 PM" src="https://user-images.githubusercontent.com/68934498/221388542-43ba2709-ee1d-4a5a-985e-cbc556716348.png">
  
> Result of push
  
Now, in the code file on Github, we can see an updated version of our code.
