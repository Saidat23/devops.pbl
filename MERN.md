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

Install the dotenv module by running the command " npm instal dotenv " 

Open the index.js file with the command " vim index.js ". 

Type in letter "I" to switch to insert mode and then type in the code below and save.

" const express = require('express');
  require('dotenv').config();

  const app = express();

  const port = process.env.PORT || 5000;

  app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "\*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
  });

  app.use((req, res, next) => {
  res.send('Welcome to Express');
  });

  app.listen(port, () => {
  console.log(`Server running on port ${port}`)
  });  "

  Click esc button and type in :w to save the code in Vim and :qa to exit Vim.

  To test if the server works, open the terminal in the same directory as the index.js file and type " node index.js "
  If everything goes well, it should read " Server running on port 5000 "
  
![Screenshot 2023-07-16 203916](https://github.com/Saidat23/devops.pbl/assets/138054715/cc1a86e3-d1b4-4721-aebf-74fba21a6000)


  Open the Security Group in your EC2 to edit the Inbound rules by creating an inbound rule to open TCP port 5000. 
  
![Screenshot 2023-08-24 174853](https://github.com/Saidat23/devops.pbl/assets/138054715/f5df2ad7-036a-4117-8c7c-835eb02dd501)

  Open the browser and try to access the server's public IP followed by port 5000. 
  i.e http://< Public IP >:5000 You should get " Welcome to Express " on your browser. 
  

![Screenshot 2023-07-17 211657](https://github.com/Saidat23/devops.pbl/assets/138054715/875a9f61-0924-4c64-8700-e57c631fc817)

This To-Do application should be able to create a new task, display list of all tasks and delete a completed task. Each task will be associated with endpoint using different http request methods: POST, GET and DELETE. For each task, we have to create route that will define various endpoints that the To-do application will depend on.

Create the routes directory using the " mkdir routes " command.

Change the directory to routes folder using "cd routes " command

Create a api.js file with the " touch api.js " command

Open the file with the command " vim api.js " then copy and paste the code below in the file. Save and exit the file.

" const express = require ('express');
  const router = express.Router();

 router.get('/todos', (req, res, next) => {

 });

 router.post('/todos', (req, res, next) => {

 });

 router.delete('/todos/:id', (req, res, next) => {

 })

 module.exports = router; ".

 #### MODELS 

 We have to create a model since we would be using Mongodb: a NoSQL database. A model makes JavaScript interactive and defines the database schema which is a blueprint of how the database would be constructed. 

 To create a Schema and a model, we would install mongoose which is a Node.js package that makes it easy to work with mongodb. 
 
 Change the directory back to To-Do folder with " cd .. " and install Mongoose with " npm install mongoose "

 Create a new folder named models by running the command " mkdir models ".

 Change directory into the newly created 'models' folder with " cd models "
 
 Inside the models folder, create a file and name it todo.js using " touch todo.js " command.

 

 
 





