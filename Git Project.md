# GIT PROJECT
  This basic Git project is a collection of beginner-friendly tasks designed to introduce you into the fundamentals of Git. Git is a powerful version control system widely used in software development.This mini project will help learners to quickly grasp the core concepts and confidently utilize Git in projects.

  Throughout the implementation, you will learn how to efficiently initialize a repository and make commits. Work with branches, collaboration, remote repositories, tagging, track changes, highlighting the significance of Git and also emphasizeing the best practices for maintaining a clean commit history, optimizing workflows and troubleshooting common issues.
## INITIALIZING A GIT REPOSITORY
  You have to install Git on your computer before initializing the Git repository. Select your desired operating system when you want to install your Git. It could be a windows, Mac or Linux operating system. To initialize the Git repository, open your prefered terminal on your computer which could  be Git bash, Visual studio, Mac terminal etc. On the terminal, create your working folder or directory using the mkdir command eg < mkdir Devops >. Change into your working directory using the command < cd Devops > then run the "git init" command while you are in the Devops directory.

  
![Screenshot 2023-08-14 224549](https://github.com/Saidat23/devops.pbl/assets/138054715/34da8e34-83e5-4f6e-9d1f-7ae63fb7c4e5)
  
## MAKING YOUR FIRST COMMIT  
  In the last section, we succesfully created our working directory and initialized a git repository. We are now going to make our first commit. In Git, commit is saving the changes made to the files which could be adding, modifying or deleting text or files. When we commit, Git takes a snapshot of the current state of the repository and saves the copy in the .git folder inside the working directory. The -m flag is used to provide a commit message which should be as descriptive as possible to give an understanding on the commit.
  
  Let's make our first commit following these steps:
    
  -- Using the touch command, create a file "index.txt" inside your working directory. eg touch index.txt.
  
  -- Write any sentence of your choice inside the text file and save changes.

  -- Add the changes to Git staging area using the command "git add ."

  -- To commit your changes to Git, run the command git commit -m "initial commit".
  
  ![Screenshot 2023-08-14 224628](https://github.com/Saidat23/devops.pbl/assets/138054715/e48d096c-e0bc-40f1-944c-bef6e0564a8c)
  
## WORKING WITH BRANCHES  
  Git branches helps to create a different copy of the source code. They are used to develop new feature of the application. As many as possible changes can be made on the new branch. All these changes are independent of what is available in the main copy.

  Git branch is an important tool for collaboration within teams, working remotely. They can make different branches while working on same feature and converge their code to one branch at the end of the day.
 
 To create a git branch, the command "git checkout -b < branch-name >" is used. The -b flag helps to create and change directory into the new branch.

  Run the command "git branch" to list the branch on your local git repository.

  To change into an old branch, run the command "git checkout < branch-name >" 

 ![Screenshot 2023-08-14 224734](https://github.com/Saidat23/devops.pbl/assets/138054715/57df6f6a-7c92-4fa3-9558-5fbd88e7f175) 

  To merge a branch into another branch, first, change into branch A than run the git command "git merge B". 

  ![Screenshot 2023-08-14 224924](https://github.com/Saidat23/devops.pbl/assets/138054715/a2380289-b065-42fa-94d9-b44b4fcc7d7d)

  To delete a branch, run the command "git branch -d < branch_name >

  ## COLLABORATION AND REMOTE REPOSITORIES

  Git, as we learnt earlier is a distributed version control system, that essentially solves the problem source code and tracking of changes made to the source code. Carrying out operations like initializing git repository in our local machine, creating commits and branches amongst others, enable collaboration amongst developers residing in different locations. For developers working remotely on the same code base on their local machine to be able to collaborate, there has to be a place where all these codes are stored for everyone involved to access. This is where Github comes in. 

  Github is a web based plateform where git repositories are hosted. Hosting our local repositories on github, makes it available in the public and can be accessed. we can also make it private which can only be accessed by selected people. Remote teams can now view update,  and make changesto the same repository.

  ### CREATING A GITHUB ACCOUNT
  Step 1. Go to www.github.com

  Step 2. Fill in your username, password and email address to creat your user account.

  Step 3. Click on the verify button to verify your identity.

  Step 4. Click on the creat button to create your account.

  Step 5. Enter the activation code sent to your email in the textboxes provided, then click on continue.

   Step 6. Select how many people will be in your team and click continue.

   Step 7. Alist of github plans will be displayed, click on continue for free.

   ### CREATING YOUR FIRST REPOSITORY
Step 1. Click on the plus sign at the top right corner of your github account. A drop down menu will appear, select new repository.


![Screenshot 2023-08-22 125610](https://github.com/Saidat23/devops.pbl/assets/138054715/f0d9d374-1c6b-4764-aaa7-0194ee5b03ec)

Step 2. Fill out the form by adding a unique name for your repository, description, you can make it public or private depending on your preference than tick the box to add a ReadMe.md file.

Step 3. Click the Create repository button. 

![Screenshot 2023-08-22 125243](https://github.com/Saidat23/devops.pbl/assets/138054715/03b82563-26e6-4df0-a516-595213f822af)

### PUSHING YOUR LOCAL GIT REPOSITORY TO YOUR REMOTE GITHUB REPOSITORY.

We have written some documents in our local git repository and our collegue is willing to contribute to it but unable to because our document is still in our local repository. For them to have access to the document in our local repository, we would have to send a copy to our repository in the github. This can be achieved by following the steps below.

Step 1. Add a remote repository to the local repository using the command " git remote add origin < link to your github repository >". To get the remote link, click on the green button code to copy the link.


![Screenshot 2023-08-14 225001](https://github.com/Saidat23/devops.pbl/assets/138054715/4565c1ff-045c-4b87-b0aa-b1441a194322)

Step 2. After commiting your changes in your local repository, push the content to the remote repository using the command " git push origin < branch name > ".

![Screenshot 2023-08-14 234254](https://github.com/Saidat23/devops.pbl/assets/138054715/b3f78de2-fda6-4c55-a759-4b9425fb5d0b)

### CLONING YOUR REMOTE GIT REPOSITORY.

We have successfully added a remote git repository and pushed our document in the local repository to the remote repository. Now our collegue can make changes to them but not directly on the github.It is best practice that he makes a copy of the document on his local machine, create a branch where he can make all modification he deems fit. To make a copy of the document, he would run the git clone command which makes a copy of remote repository into local machine. Run the command "git clone < link to your remote repository >".


![Screenshot 2023-08-14 234314](https://github.com/Saidat23/devops.pbl/assets/138054715/27d31f7c-30c1-4df2-911f-b51fec9c80b4)






   

  
  

