# MERN STACK IMPLEMENTATION

In this project, we would be implementing a web solution based on MERN stack in AWS Cloud.

MERN Web stack consists of:

MongoDB: is a document-based, No-SQL database, used to store application data in form of documents.

ExpressJS: is a server side Web Application framework for Node.js.


ReactJS: is a frontend framework developed by Facebook. It is based on JavaScript and used to build User Interface (UI) components.


Node.js: is a JavaScript runtime environment. It is used to run JavaScript on a machine.


The diagram below illustrate how the MERN Stack works.
<img width="638" alt="MERN-stack" src="https://github.com/Saidat23/devops.pbl/assets/138054715/5cde0948-b568-4b85-80ef-62dbfe64a652">


When a user interacts with the ReactJS UI components at the application front-end in the browser. The application backend in the server respond to the frontend through ExpressJS running on top of NodeJS.

Any interaction that causes a data change request is sent to the NodeJS based Express server, which request for data from the MongoDB database. The data received is sent back to the frontend application, and presented to the user.

## DEPLOYING A SIMPLE APPLICATION THAT CREATES A TO-DO LIST.
### Step 1. BACKEND CONFIGURATION
Update Ubuntu by runing the command " sudo apt update "

Upgrade Ubuntu by runing the command " sudo apt upgrade "

To check the location of the Node.js software run the command " curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - "


#### Installing Node.js on the server.


Install Node.js and npm with the command " sudo apt-get install -y nodejs " .
NPM is a Node package manager used to install Node modules and packages and also to manage dependency conflicts.

Verify the node installation by runing the command " node -v ".

Verify the npm installation by runing the command " npm -v ".

#### Application Code Setup.

Create a new directory for the To-Do Project by running the command " mkdir Todo "

Verify the Todo directory by runing the command " ls ".

Change the current directory to the new directory by runing the command " cd Todo ".

When we initialise the project by runing the command " npm init " , a new file named package.json will be created. This file contain the information about the application and the dependencies it needs to run. Follow the prompts after runing the command. You might have to press Enter several times to accept default values, then type yes to accept to write out the package.json file. 

Run the " ls " command to verify that you have the package.json file created.


#### Installing ExpressJS
Express is a framework for Node.js, it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of the application based on HTTP methods and URLs.

Install ExpressJS with the command " npm install express "

Create a file named index.js with the commamnd " touch index.js " 

Run " ls " command to confirm that the file is created.

Install the dotenv by running the command " npm instal dotenv "







