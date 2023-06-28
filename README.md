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

10-


___________________________
[static]
1-npm run generate 2-zip dist 3-upload to public 4-done

