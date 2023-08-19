# GIT PROJECT
  This basic Git project is a collection of beginner-friendly tasks designed to introduce you into the fundamentals of Git. Git is a powerful version control system widely used in software development.This mini project will help learners to quickly grasp the core concepts and confidently utilize Git in projects.

  Throughout the implementation, you will learn how to efficiently initialize a repository and make commits. Work with branches, collaboration, remote repositories, tagging, track changes, highlighting the significance of Git and also emphasizeing the best practices for maintaining a clean commit history, optimizing workflows and troubleshooting common issues.
## INITIALIZING A GIT REPOSITORY
  You have to install Git on your computer before initializing the Git repository. Select your desired operating system when you want to install your Git. It could be a windows, Mac or Linux operating system. To initialize the Git repository, open your prefered terminal on your computer which could  be Git bash, Visual studio, Mac terminal etc. On the terminal, create your working folder or directory using the mkdir command eg <mkdir Devops>. Change into your working directory using the command (cd Devops) then run the "git init" command while you are in the Devops directory.
## MAKING YOUR FIRST COMMIT  
  In the last section, we succesfully created our working directory and initialized a git repository. We are now going to make our first commit. In Git, commit is saving the changes made to the files which could be adding, modifying or deleting text or files. When we commit, Git takes a snapshot of the current state of the repository and saves the copy in the .git folder inside the working directory. The -m flag is used to provide a commit message which should be as descriptive as possible to give an understanding on the commit.
  
  Let's make our first commit following these steps:
    
  -- Using the touch command, create a file "index.txt" inside your working directory. eg touch index.txt.
  
  -- Write any sentence of your choice inside the text file and save changes.

  -- Add the changes to Git staging area using the command "git add ."

  -- To commit your changes to Git, run the command git commit -m "initial commit".
## WORKING WITH BRANCHES  
  Git branches helps to create a different copy of the source code. They are used to develop new feature of the application. As many as possible changes can be made on the new branch. All these changes are independent of what is available in the main copy.

  Git branch is an important tool for collaboration within teams, working remotely. They can make different branches while working on same feature and converge their code to one branch at the end of the day.
 
 To create a git branch, the command "git checkout -b <branch-name>" is used. The -b flag helps to create and change directory into the new branch.

  Run the command "git branch" to list the branch on your local git repository.

  To change into an old branch, run the command "git checkout <branch-name>" 

  To merge a branch into another branch, first, change into branch A than run the git command "git merge B". 

  To delete a branch, run the command "git branch -d <branch_name>

  ## COLLABORATION AND REMOTE REPOSITORIES



   

  
  

