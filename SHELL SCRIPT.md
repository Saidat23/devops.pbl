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
   Bash provides control flow statements like **if-else**, **for loops**, **while loops** and **case statements** to control the flow of execution in the scripts. These statements allows you to make decisions, iterate over lists and execute different commands based on conditions. <br />
   #### Using if-else.
   The code below prompt for a number and print if the number is positive, negative or zero.<br />
  ```
   #!/bin/bash

# Example script to check if a number is positive, negative, or zero

read -p "Enter a number: " num

if [ $num -gt 0 ]; then
    echo "The number is positive."
elif [ $num -lt 0 ]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi
  ```
#### Iterating through a list using for loop.
  ```
#!/bin/bash

# Example script to print numbers from 1 to 5 using a for loop

for (( i=1; i<=5; i++ ))
do
    echo $i
done
  ```
3. ### COMMAND SUBSTITUTION
   Command substitution captures the output of a command and use it as a value within the script. Both the bascktick **`** and the **$()** sysntax can be used.<br />
   Using the backtick for command substitution.
   
   ``` current_date=`date +%Y-%m-%d`  ```
   
   Using the **$( )** for command substitution.

   ```current_date=$(date +%Y-%m-%d)```
   
4. ### INPUT AND OUTPUT
    In bash, the **read** command is used to accept user input while the **echo** command is used to print out the text. The input and output can be redirected using operators like **>** (output to a file), **<** (input to a file) and **|** (pipe the output of one command as input to another).<br />
    #### Accept user input
   
   ```
    echo "Enter your name:"
        read name
   ```

   ![image](https://github.com/Saidat23/devops.pbl/assets/138054715/b47705ac-ffd9-4b9a-ba5e-54f20936259a)

   #### Output text to the terminal

   ``` echo "Hello World" ```
   
   ![image](https://github.com/Saidat23/devops.pbl/assets/138054715/1489486f-3e23-4c72-bedf-738caf956454)

   #### Output the result of a command into a file
   
   ``` echo "hello world" > index.txt ```

   ![image](https://github.com/Saidat23/devops.pbl/assets/138054715/7366a274-7df4-4c1c-abe3-3ed547e0c85e)

  #### Pass the result of a file as input to a command
  
  ``` grep "pattern" < input.txt ```

  #### Pass the result of a command as input to another command
  
  ``` echo "hello world" | grep "pattern" ```
  
  5. ### FUNCTIONS
     




