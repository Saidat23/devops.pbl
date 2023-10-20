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
     Functions are used to group related commamds together. It provides a way to modularize codes and make it more reusesable.Functions can be defined using the function keyword or by simply declaring the function name followed by parentheses.
     
```
     #!/bin/bash

# Define a function to greet the user
greet() {
    echo "Hello, $1! Nice to meet you."
}

# Call the greet function and pass the name as an argument
greet "John"

```

![image](https://github.com/Saidat23/devops.pbl/assets/138054715/c29bb0e2-de82-4cc9-bdb5-614744176637)

## SHELL SCRIPTING PROJECT
---
Open a folder on the terminal with the name **shell-scripting** using the command below

``` mkdir shell-scripting ```
    
Create a file named **user-input.sh** using the command

``` touch user-input.sh ```

![Screenshot 2023-10-18 132552](https://github.com/Saidat23/devops.pbl/assets/138054715/ebfbbeb1-0a2a-4686-97de-c56fa9618bad)

Inside this file, copy and paste the block of code below

```
#!/bin/bash

# Prompt the user for their name
echo "Enter your name:"
read name

# Display a greeting with the entered name
echo "Hello, $name! Nice to meet you."

```
![Screenshot 2023-10-18 130953](https://github.com/Saidat23/devops.pbl/assets/138054715/c48b955b-659b-454f-92f4-633ee2c7537d)

Save the file and exit nano text editor.<br />
This script prompt for your name. When you type in your name, it displays the text Hello, (your name)! Nice to meet you.


Run the command below on the terminal to make the file executable.

``` sudo chmod +x user-input.sh ```

Then run the script on the terminal using the command 
``` ./user-input.sh ```


![image](https://github.com/Saidat23/devops.pbl/assets/138054715/a9868d20-5f0e-42ce-acb9-fd59107a7851)

### Directory Manipulation and Navigation
This script will display the current directory, create a new directory named **my_directory**, change to the new directory, create two files inside it, list the files then move back one level up, remove the 
**my_directory** and its contents, then list the files in the current directory again. <br />

  **Step 1**: Create  a file named navigation-linux-filesystem.sh <br />
  **Step 2** :  Open the file and paste the code block below into your file <br />
  ```
    #!/bin/bash

# Display current directory
echo "Current directory: $PWD"

# Create a new directory
echo "Creating a new directory..."
mkdir my_directory
echo "New directory created."

# Change to the new directory
echo "Changing to the new directory..."
cd my_directory
echo "Current directory: $PWD"

# Create some files
echo "Creating files..."
touch file1.txt
touch file2.txt
echo "Files created."

# List the files in the current directory
echo "Files in the current directory:"
ls

# Move one level up
echo "Moving one level up..."
cd ..
echo "Current directory: $PWD"

# Remove the new directory and its contents
echo "Removing the new directory..."
rm -rf my_directory
echo "Directory removed."

# List the files in the current directory again
echo "Files in the current directory:"
ls
```
**Step 3**: Run the sudo command to set execute permission on the file.<br />

``` sudo chmod +x navigating-linux-filesystem.sh ```

**Step 4**: Run the script using the command

``` ./navigating-linux-filesystem.sh ```
















    



