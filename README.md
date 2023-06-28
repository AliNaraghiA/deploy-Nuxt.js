# deploy-Nuxt.js
How to deploy a Nuxt.js v2 in cPanel that supports Node.js (ssr,ssc)
______________________________________________________________________Nuxt project
[univerall] 
1-first, test locally:
npm run build
npm run start
ok!
2-zip ->  upload the .nuxt directory ,static,assets , and the other files (nuxt.config.js, package.json, package-lock.json, and server/index.js if exists) api (if u have).
@ pacakge.json : 
Remember, the "start": "nuxt start" script in your package.json file should be used to start your application on the server, not the "build": "nuxt build" script. The "build": "nuxt build" script is only used to build your application for production on your local development environment.
______________________________________________________________________Cpanel
2/5:You should create the Node.js application in cPanel before uploading your .nuxt directory and other necessary files.
select correct node v and root then >>application startup file (typically app.js or server.js).

--
But in the context of a Nuxt.js application, the main entry point is typically nuxt.config.js, where you define the configuration for your Nuxt application, and server/index.js where you define your custom server strapi.io.

Here's an example of a Nuxt.js server setup:

// server/index.js
const express = require('express')
const consola = require('consola')
const { Nuxt, Builder } = require('nuxt')
const app = express()

// Import and Set Nuxt.js options
let config = require('../nuxt.config.js')
config.dev = !(process.env.NODE_ENV === 'production')

async function start () {
  // Init Nuxt.js
  const nuxt = new Nuxt(config)

  const { host, port } = nuxt.options.server

  await nuxt.ready()
  // Build only in dev mode
  if (config.dev) {
    const builder = new Builder(nuxt)
    await builder.build()
  }

  // Give nuxt middleware to express
  app.use(nuxt.render)

  // Listen the server
  app.listen(port, host)
  consola.ready({
    message: `Server listening on http://${host}:${port}`,
    badge: true
  })
}
start()
In the above code, we are using Express with Nuxt.js to create a server. The server is then started with the Nuxt configuration and listens for incoming requests.
--
3- upload the zipped file to your cPanel account. upload the file to the root directory of your domain or subdomain. After uploading, extract the zip file using the "Extract" option in the cPanel File Manager.
4-In the root directory, there will already be some files like .htaccess, app.js, package.json, and package-lock.json.

.htaccess: You need to modify this file to route requests to your Node.js application. Add the following lines to your .htaccess file:
5-Configure .htaccess:Replace <your Port> with the port number of your Node.js application::
In the public_html directory or the directory for your subdomain, create or modify the .htaccess file. Add the following lines to route all requests to your Node.js application:
   Options +FollowSymLinks -Indexes 
   IndexIgnore * 
   DirectoryIndex 
   <IfModule mod_rewrite.c> 
   RewriteEngine on 
   RewriteRule ^(.*)$ http://localhost:<your Port>/\$1 [P] 
   </IfModule>
6-app.js: This is the main file of your Node.js application. If you have a custom server setup, replace the content of this file with your server setup.
7- package.json and package-lock.json: These files are used to manage the dependencies of your Node.js application. The uploaded versions of these files from your Nuxt.js application should replace the existing ones.
8-Creating a Node.js Application in cPanel

Go to the "Software" section in cPanel, then click on "Setup Node.js App". Create a new application, select the Node.js version that matches your local development environment, and set the application root and application URL. The application root should be the directory where you uploaded your Nuxt.js application files.
9- Installing Dependencies and Starting the Application

In the Node.js app setup in cPanel, click on "Enter the virtual environment". This will open a terminal where you can run commands for your application.

Run npm install to install the dependencies of your application.
Run npm run start to start your application nuxtjs.org.

10-app.js is typically the main file of your Node.js application that starts your server. It is usually the script you would run to start your application, like node app.js.
11-PM2 is a process manager for Node.js applications. It keeps your application running even if the application crashes or the server reboots. You can install it globally with npm install pm2 -g.
12-ecosystem.config.js is a configuration file for PM2. It allows you to define various settings for your application, such as the script to start your application, the environment variables to use, and more. You can start your application with PM2 using this configuration file with pm2 start ecosystem.config.js.
--
cd in to the project directory if you are not already in there
Create a file called “ecosystem.config.js” in your project’s root folder this is config
module.exports = {
    apps: [
        { 
           name: ‘name of app',
           exec_mode: 'cluster',
           instances: 'max', // Or a number of instances
           script: './node_modules/nuxt/bin/nuxt.js',
           args: 'start',
           env: {
               NODE_ENV: "production",
               HOST: '0.0.0.0',
               PORT: 35000
           }
        }
    ]
}
      run pm2 start to start application with PM2
--
-
Update package.json
Update your package.json file to include the host and port in the config section and add a deploy script. Replace customport with your actual port and yourdomain with your actual domain.

   "config": {
     "nuxt": {
       "host": "0.0.0.0",
       "port": "customport"
     }
   },
   "scripts": {
     "dev": "nuxt --host yourdomain --port customport",
     "build": "nuxt build",
     "start": "nuxt start",
     "generate": "nuxt generate",
     "deploy": "/opt/cpanel/ea-nodejs10/bin/pm2 start /opt/cpanel/ea-nodejs10/bin/npm --name yourpm2name -- start"
   },

___________________________
[static]
1-npm run generate 2-zip dist 3-upload to public 4-done

