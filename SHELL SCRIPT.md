# INTRODUCTION TO SHELL SCRIPTING AND USER INPUT
---
## SHELL SCRIPTING.

Writing out commands on the terminal to carry out task and getting corresponding output can be challenging when we have large number of tasks to carry out. Say we have thousands of repositories to clone. It would take a lot of time and energy to clone the repository one after the other and there might be human error due to fatigue.
To prevent human error, we make use of shell scripting. Shell scripting helps to automate repetitive task by simply writing a script which does the tasks once it is called. this script can be used severally whenever it is needed to carry out the same task.
Bash scripts are series of commands and instructions that are executed sequentially in a shell. Shell script is an important part of process automation in linux. It is writen by saving sequence of commands in a text file with a **.sh** extension which is then executed directly from the command line or called from other scripts. This saves time because we don't have to write the commands again and again. We can perform daily tasks efficiently and even schedule tasks for automatic execution. We can also set certain scripts to execute on startup or set certain environment variables. 
## SHELL SCRIPTING SYNTAX ELEMENTS.
1. ### VARIABLES
    Variables store data of various types such as numbers, strings and arrays. Values can be assigned to variables using the **= opreator** and access their values using the variable name preceded by a **$** sign. <br />
We can define a variable by using the syntax **variable_name=value** <br />

```
 #! /bin/bash
# Variable example
greeting=Hello
name=John
echo $greeting $name
```


2. ### CONTROL FLOW
   Bash provides control flow statements like **if-else**, **for loops**, **while loops** and **case statements** to control the flow of execution in the scripts. 
