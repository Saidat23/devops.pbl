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
**Step 1**: Open a folder on the terminal with the name **shell-scripting** using the command below

``` mkdir shell-scripting ```
    
**Step 2**: Create a file named **user-input.sh** using the command

``` touch user-input.sh ```

![Screenshot 2023-10-18 132552](https://github.com/Saidat23/devops.pbl/assets/138054715/ebfbbeb1-0a2a-4686-97de-c56fa9618bad)

**Step 3**: Inside this file, copy and paste the block of code below

```
#!/bin/bash

# Prompt the user for their name
echo "Enter your name:"
read name

# Display a greeting with the entered name
echo "Hello, $name! Nice to meet you."

```
![Screenshot 2023-10-18 130953](https://github.com/Saidat23/devops.pbl/assets/138054715/c48b955b-659b-454f-92f4-633ee2c7537d)

**Step 4**: Save the file and exit nano text editor.<br />

This script prompt for your name. When you type in your name, it displays the text Hello, (your name)! Nice to meet you.

**Step 5**: Run the command below on the terminal to make the file executable.

``` sudo chmod +x user-input.sh ```

**Step 6**: Then run the script on the terminal using the command 

``` ./user-input.sh ```


![image](https://github.com/Saidat23/devops.pbl/assets/138054715/a9868d20-5f0e-42ce-acb9-fd59107a7851)

### Directory Manipulation and Navigation

This script will display the current directory, create a new directory named **my_directory**, change to the new directory, create two files inside it, list the files then move back one level up, remove the 
**my_directory** and its contents, then list the files in the current directory again. <br />

  **Step 1**: Create  a file named **navigation-linux-filesystem.sh** <br />
  **Step 2** :  Open the file with a text editor and paste the code block below into the text <br />

  ![Screenshot 2023-10-18 133434](https://github.com/Saidat23/devops.pbl/assets/138054715/712eb5a4-cf6e-4ad9-a28c-e02f243d94f9)
  
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

![Screenshot 2023-10-18 133124](https://github.com/Saidat23/devops.pbl/assets/138054715/111f9db7-1cf2-4ef3-837d-812156526f94)

**Step 3**: Save and Exit the text editor. <br />

**Step 4**: Run the sudo command to set execute permission on the file.<br />

``` sudo chmod +x navigating-linux-filesystem.sh ```

**Step 5**: Run the script using the command

``` ./navigating-linux-filesystem.sh ```

 ![Screenshot 2023-10-18 133505](https://github.com/Saidat23/devops.pbl/assets/138054715/b8e3428b-bb9f-4ffb-8847-7ed4f8f8cc0b)

### File Operations and Sorting
We would be writing a simple shell script that focuses on file operations and sorting. <br />
In this script, we would create three files **(file 1.txt, file2.txt and file3.txt)**. We would display the files in their current order, sort them alphabetically, save the sorted files in **sorted_file.txt**, display the sorted files, remove the original files, rename the sorted file to **sorted_files_sorted_alphabetically.txt** then finally display the contents of the final sorted file.<br />

**Step 1**: Create a file with the name **sorting.sh** on your terminal with the command

``` touch sorting.sh ```

**Step 2**: Open the file with a text editor using the command

``` nano sorting.sh ```

**Step 3**: Paste the code block below into the text file.

``` 
#!/bin/bash

# Create three files
echo "Creating files..."
echo "This is file3." > file3.txt
echo "This is file1." > file1.txt
echo "This is file2." > file2.txt
echo "Files created."

# Display the files in their current order
echo "Files in their current order:"
ls

# Sort the files alphabetically
echo "Sorting files alphabetically..."
ls | sort > sorted_files.txt
echo "Files sorted."

# Display the sorted files
echo "Sorted files:"
cat sorted_files.txt

# Remove the original files
echo "Removing original files..."
rm file1.txt file2.txt file3.txt
echo "Original files removed."

# Rename the sorted file to a more descriptive name
echo "Renaming sorted file..."
mv sorted_files.txt sorted_files_sorted_alphabetically.txt
echo "File renamed."

# Display the final sorted file
echo "Final sorted file:"
cat sorted_files_sorted_alphabetically.txt
```
![Screenshot 2023-10-18 134850](https://github.com/Saidat23/devops.pbl/assets/138054715/3c53978b-a804-4dd8-b4bf-8fd18f2c0394)

**Step 4**: Save and Exit the text file with **ctrl O** followed by **Enter** the  **ctrl X** <br />

**Step 5**: Set the execution permission on sorting.sh with the command <br />

``` sudo chmod +x sorting.sh ```

**Step 6**: Run the script with the command 

``` ./sorting.sh ```


![Screenshot 2023-10-18 135217](https://github.com/Saidat23/devops.pbl/assets/138054715/d8d67d96-db63-4373-adc4-56ab73feac44)

### Working with Numbers and Calculations

This script defines two variables Num1 and Num2 with numerical values. It performs basic arithemetic operations and displayys the results. It also carry out complex calculations like squaring and finding the square root of a number.

**Step 1**: On your terminal, create a file named **calculations.sh** with the command

``` touch calculations.sh ```

**Step 2**: Open the file with a text editor using the command

``` nano calculations.sh ```

**Step 3**: Copy and paste the code block below into the text editor.

```
 #!/bin/bash

# Define two variables with numeric values
num1=10
num2=5

# Perform basic arithmetic operations
sum=$((num1 + num2))
difference=$((num1 - num2))
product=$((num1 * num2))
quotient=$((num1 / num2))
remainder=$((num1 % num2))

# Display the results
echo "Number 1: $num1"
echo "Number 2: $num2"
echo "Sum: $sum"
echo "Difference: $difference"
echo "Product: $product"
echo "Quotient: $quotient"
echo "Remainder: $remainder"

# Perform some more complex calculations
power_of_2=$((num1 ** 2))
square_root=$(echo "num2" | awk '{print sqrt ($1)}')

# Display the results
echo "Number 1 raised to the power of 2: $power_of_2"
echo "Square root of number 2: $square_root"
```
![Screenshot 2023-10-18 141116](https://github.com/Saidat23/devops.pbl/assets/138054715/4ac11dfd-fe8d-4e4d-80ee-8997ba4847dc)

**Step 4**: Save and Exit the text file with **ctrl O** followed by **Enter** the  **ctrl X**

**Step 5**: Set execute permission on the calculations.sh using the command

``` sudo chmod +x calculations.sh ```

**Step 6**: Run the script with the command

``` ./calculations.sh ```

![Screenshot 2023-10-18 141054](https://github.com/Saidat23/devops.pbl/assets/138054715/0ae66524-e9ed-4c64-92ae-889aa993ee48)

### File Backup and Timestamping.

As a DevOps engineer, backing up databases and other storage devices is one of the most common task you carry out. This shell scripting is focused on file backup and timestamp. It defines the source directory and the backup directory paths then create a timestamp using the current date and time. It creates a backup directory with the timestamp appended to its name. The script then copy all files from the source directory to the backup directory using the **cp command** with the **-r** option for recursive copying. It finally displays a message indicating the completion of the backup process and shows the backup directory path with the timestamp.

**Step 1**: On your terminal, create a file **backup.sh** with the command

``` touch backup.sh ```

**Step 2**: Open the file with a text editor, then copy and paste the code block below into the text editor.

```
#!/bin/bash

# Define the source directory and backup directory
source_dir="/path/to/source_directory"
backup_dir="/path/to/backup_directory"

# Create a timestamp with the current date and time
timestamp=$(date +"%Y-%m-%d-%H-%M-%S")

# Create a backup directory with the timestamp
backup_dir_with_timestamp="$backup_dir/backup_$timestamp"

# Create the backup directory
mkdir -p "$backup_dir_with_timestamp"

# Copy all files from the source directory to the backup directory
cp -r "$source_dir"/* "$backup_dir_with_timestamp"

# Display a message indicating the backup process is complete
echo "Backup completed. Files copied to: $backup_dir_with_timestamp"
```

![Screenshot 2023-10-18 141339](https://github.com/Saidat23/devops.pbl/assets/138054715/4de290eb-b24f-4ee2-bf80-1d8b0dcdaccd)

**Step 3**: Save and Exit the text file with **ctrl O** followed by **Enter** the  **ctrl X**
  
**Step 4**: Set execute permission on the backup.sh using the command

``` sudo chmod +x backup.sh ```

**Step 5**: Run the script with the command

``` ./backup.sh ```

![Screenshot 2023-10-18 142432](https://github.com/Saidat23/devops.pbl/assets/138054715/f24dabc5-ade5-4189-9d15-169d0638d27a)





























































    



