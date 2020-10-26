# New Node Project
## Create a new directory
- mkdir as usual
- cd into directory
## Initialize Node inside the directory
- **IMPORTANT** - Make sure you're in the correct directory!
- npm init
    - Hit ENTER through all the prompts
- **npm init -y** will skip this

## Make your entry point
- touch index.js (unless you named it something else)

## Run it
- node index.js

# Node Modules
## Create modules
- module.exports.whatever **IMPORTANT**
```javascript
    const myModule = require('./myModule.js')
```

## Built-in modules
- fs
    - read a txt file or json file & turn into a string
    - (https://www.w3schools.com/nodejs/ref_fs.asp)
```javascript
    let dinosaurs = fs.readFileSync('./dinosaurs.json'); // read the json file, turn into string
    let dinoData = JSON.parse(dinosaurs); // convert the string to an array
```
- HTTP
    - create an HTTP server
    ```javascript
        const http = require('http')
        http.createServer(function (req, res) {
            res.write('Hello World!')
            res.end()
            }).listen(8000)
    ```
    - IN BROWSER: http://localhost:8000/
- Moment
    - Time & Date module
    ```javascript
    const moment = require('moment')
    // Format date // Today is default
    // Do adds -th, DD gives number
    console.log(moment().format('MMM Do YYYY'))
    ```
- urlencoded
    - body-parser middleware
```javascript
    app.use(express.urlencoded({extended: false}))
```

# Node Packages
## To install a node package
- npm i packageName
- npm i -g packageName (install globally)
## To uninstall a node package
- npm uninstall packageName

### If other packages are needed, they will also install as "dependencies" (only for locally-installed packages)
- Useful when other users need to run your code 

## To run an installed package
- type packageName into terminal
- ctrl + C to stop running

## Put node_modules in a .gitgnore file
- list the directory or file name and git won't upload it
- add anything else you don't want to upload to github (large files, passwords, sensitive info, etc)

## 'Port already in use' troubleshooting
- killall node - end all node processes if getting errors
- If you’re getting ‘port already in use’ errors, and you’re on a mac, you can kill the server with these commands:
- in a Bash terminal (not node) run lsof -i tcp:3000 which will tell you your PID
- then you can use that to kill your server with kill -9 10020 (except instead of 10020 you’ll use whatever PID you get from the first command).
- npx kill:port "port number"

# Express
- npm i express
#### Import the express module
```javascript
    const express = require('express')
```
#### Create an instance of the express app
```javascript
    const app = express()
```
### localhost ports
    app.listen(8000, () => {
        console.log('Port 8000 is OPEN')
    })

- run nodemon to connect

## Routes

#### Home Route
    app.get('/',(req,res)=>{
        res.send('Hi there!')
    })
- the '/' tells it where to start
- only one res.send or res.render per route
- order matters! higher up = running first

**Good to remember: URLs are strings!**
- Use parseInt() if you need numbers to be numbers
- some operators (like *) will convert automatically because you can't multiply strings

## Views
### Absolute file path

    __dirname+'filepath'
In express:
```javascript
    app.get('/',(req,res)=>{
        res.sendFile(__dirname+'/views/index.html')
    })
```

# EJS
(EJS = "embedded javascript")
```javascript
    npm i ejs
    // app.set(name, value)
    app.set('view engine','ejs')
```
## EJS lets us render templates that display as webpages
    res.render('index.ejs')
- ejs knows to look in the views folder automatically
## Can declare variables in template
    res.render('index.ejs', {name: 'Sterling Archer', age: 35})
## Use EJS in HTML
    <h1>Hi there, <%= name %>!</h1>
    <% let dogAge =age*7 %>
    <h2>You are <%= dogAge %> in dog years!</h2>
**You don't need to wrap HTML elements in alligator tags**
```javascript
    app.get('/about',(req,res)=>{
        res.render('about.ejs', {pets: ['cora','tali','eloise']})
    })
```
    <ul>
        <% pets.forEach(pet => { %>
            <li>
            <%= pet %>
            </li>
        <% }) %>
    </ul>

## Partials
reusable "partial" pieces of code
- mkdir partials
- touch partials/header.ejs

- In main files
```javascript
    <%- include('../partials/header.ejs') %>
```

## Static Files
- link CSS, images, html, etc pages to express projects
- mkdir public
- mkdir css (or images, etc)
- touch public/css/style.css
- in index.js
```javascript
    app.use(express.static('public'))
```
- in layout.ejs head (for css)
```javascript
    <link rel="stylesheet" href="http://localhost:<%= process.env.PORT %>/css/style.css"> 
```
or (for img)
```javascript
   <img src="http://localhost:3000/images/leo.jpg"/>
```

# Express-EJS-Layouts
- npm i express-ejs-layouts
```javascript
    const express = require('express')
    const app = express()
    const ejsLayouts = require('express-ejs-layouts')

    app.set('view engine', 'ejs')
    app.use(ejsLayouts)

    app.listen(3000, ()=> {
        console.log('Port 3000 today')
    })
```
- touch views/layouts.ejs
    - anything you put in this file will appear on all your pages
    - saves time not having to set up every single view
    - set up HTML boilerplate, connect static files, etc
    ```javascript
        <%- body %> // this is where the individual page data goes
    ```
- make other EJS pages as usual

## Controllers
- add a "controllers" directory
- makes branches from home route
- instead of "app.get" it becomes "router.get"
- need to redeclare variables that you're using on controller pages
```javascript
   const express = require('express')
   const router = express.Router() // this is the one for controller pages
   const fs = require('fs')
```
- at bottom of page include
```javascript
    module.exports = router
```
- on index.js (or whatever), require and use your controllers
```javascript
    const dinosaurs = require('./controllers/dinosaurs')
    app.use('/dinosaurs',dinosaurs) // this sets up having the controller name in front of its addresses
```

## Method Override
- lets you override form methods
- HTML5 won't let you use PUT directly to avoid malware, so you have to go around

# Axios
- similar to fetch()
- has more browser support than fetch()
- axios automatically stringifies the data when sending requests (don't need response => response.json)

# dotenv
- allows you to use a .env file to turn sensitive data into variables
- list .env on .gitignore

# Sequelize
- we installed sequelize-cli globally
- have to install pg & sequelize to run locally
- sequelize init to run - makes a bunch of directories
- have to update config.json with postgres info
- until you change your postgres password, put config.json into .gitignore, dummy
### Set Up
- sequelize init
- update config.json with username, password, database names, & dialect (postgres)

## Working in Sequelize
**SPACES ARE THE ENEMY. DESTROY (almost) ALL SPACES**
#### In terminal
- sequelize db:create DATABASENAME
- sequelize model:create --name user --attributes firstName:string,lastName:string,age:integer,email:string
- sequelize db:migrate /db:migrate:undo
    - If you make a typo on one of your attributes, unmigrate (**WARNING** you will lose any data that's already in the table), correct in the model AND in the migration, re-migrate

#### In index.js (or whatever)
    const db = require('./models')
    // **CREATE**
    db.user.create({
        firstName: 'Andy',
        lastName: 'Hughes',
        age: 33
        })
    .then(createdUser => {
    // the create promise returns the new row of data that has been created in the database, otherwise it throws an error -->
        console.log('Created User:', createdUser)
    })
    .catch(err => {
        console.log(err)
    })

    // **READ (ALL)**
    db.user.findAll().then(foundUsers => {
        console.log('Found Users:', foundUsers) 
        // returns in an array
        })

    // **READ (ONE)**
    db.user.findOne({
        where: {firstName: 'Andy'}
    }).then(foundUser => {
        console.log('Found User:',foundUser)
    })


    // **CREATE OR READ**
    db.user.findOrCreate({ 
        // first searches, then creates if not found
        where: {
            firstName: 'Cora'
        },
        defaults: {
            email: 'WatIzEnail@AHHHHHH.boo',
            age: 0.5
        }
    })
    .then(([foundOrCreatedUser, created])=>{
        console.log('Found or Created User:', foundOrCreatedUser)
        console.log('Already in database?', !created)
    })

    // **UPDATE (ONE or MANY)**
    db.user.update({
        lastName: 'Bear'
    },
    {
        where: {
            firstName: 'Tali'
         }
    }).then(numRowsChanged => { 
        // you can update multiple rows, so you can check how many were affected
        console.log(numRowsChanged)
    })


    // **DESTROY**
    db.user.destroy({
        where: {firstName: 'Andy'}
    })
    .then(numRowsDeleted => {
        console.log(numRowsDeleted)
    })