# Week 1 Lab Report

Name: Harsh Gurnani

PID: A17342880

Email: hgurnani@ucsd.edu

This guide contains instructions for UCSD students to log into a course specific account on ieng6. Students can use Visual Code Studio, a code editor, to connect to the server remotely and run commands. The tutorial will dive into all the details of how to do so. 

In this guide, we will cover three main points: 
* Installing Visual Studio Code
* Remotely connecting to the server
* Running commands on the command line

---

## Installing Visual Studio Code

Visual Studio Code, also known as VS Code, is a widely used code editor. It has a lot of great features for programmers, such as providing suggestions, a debugger, and more. You can download VS Code at [https://code.visualstudio.com/](https://code.visualstudio.com/). 


<img width="1679" alt="Screen Shot 2023-01-14 at 7 19 24 PM" src="https://user-images.githubusercontent.com/68934498/212521322-78fddc16-3c5e-4f41-93fa-fe65e2f03277.png">

> Downloading page for VS Code


When you visit the site, you will see a screen like the one above. Make sure to follow the instructions for installing the correct version of VS Code, depeending on if you have a Mac or Windows; you can use the download button at the top right. For my uses, I simply had to click on "Download Mac Universal Build."

After installing VS Code, you should open the app and see a screen like this: 

<img width="1025" alt="Screen Shot 2023-01-12 at 10 14 56 AM" src="https://user-images.githubusercontent.com/68934498/212521436-0156f8dd-60c2-4591-badb-a377a4bc091e.png">

> Launch page for VS Code 

Now, you can open your files directly in the application. 

## Remotely Connecting to the Server

A prerequisite to being able to connect remotely is having your CSE 15L account. At [https://sdacs.ucsd.edu/~icc/index.php](https://sdacs.ucsd.edu/~icc/index.php), log in at the top where it says Account Lookup. 

<img width="604" alt="Screen Shot 2023-01-14 at 7 32 39 PM" src="https://user-images.githubusercontent.com/68934498/212521636-3adbe9b5-8bd2-47f3-a362-effd060cea0a.png">

> Log in to access CSE 15L account

Under additional accounts, you should see a button with a text beginning with "cs15lwi23". That entire text is your CSE 15L account username. 

On a Mac, you can directly go to the terminal on VS Code (on Windows, you may have to install git at [https://gitforwindows.org/](https://gitforwindows.org/)). Make sure you have your account username for the CSE15L class on hand. In the terminal, run the command:
```
# ssh *cse15lusername*@ieng6.ucsd.edu
```
You should end up with a screen that says:
```
# The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```
Click yes, and type in your password.

Following the login, your terminal will print a lot of messages, and then will show your usage statistics of the CPU. Mine currently looks like this: 

<img width="344" alt="Screen Shot 2023-01-14 at 7 38 40 PM" src="https://user-images.githubusercontent.com/68934498/212521785-634175e8-3ee5-4f93-8909-e80253664142.png">

> Terminal text after connecting to remote SSH

At this point, you are connected remotely and can run commands on the computer that you are connected to from your own laptop's terminal. 

## Running Commands on the Command Line

There are a lot of common commands that all programmers should know. You can try these both by connecting to the server or just using a local directory on your laptop. Here are some of them:
* pwd - writes the pathname of your current directory
* cd - changes your directory to a new path; you can input either an absolute or relative path name
* cd .. - goes up to a parent directory
* cd ~ - goes up to the home directory
* ls - lists all the files and folders under the current directory you are in
* mkdir - makes a new directory at a defined path

On my laptop, it looks like this:

<img width="667" alt="Screen Shot 2023-01-14 at 7 51 09 PM" src="https://user-images.githubusercontent.com/68934498/212522038-e177b289-a990-42aa-881b-ffa7a92f2bda.png">

> Example of using basic commands on terminal


And that's all the basics! Remember to reach out to your professor or TA's if you need any help. 
