# deploy-Nuxt.js
How to deploy a Nuxt.js v2 in cPanel that supports Node.js (ssr,ssc)
______________________________________________________________________Nuxt project
[univerall] 
1-first, test locally:
npm run build
npm run start
ok!
2-zip ->  upload the .nuxt directory ,static,and the other files (nuxt.config.js, package.json, package-lock.json, and server/index.js if exists) api (if u have).
@ pacakge.json : 
Remember, the "start": "nuxt start" script in your package.json file should be used to start your application on the server, not the "build": "nuxt build" script. The "build": "nuxt build" script is only used to build your application for production on your local development environment.

______________________________________________________________________Cpanel
1-Configure .htaccess:Replace <your Port> with the port number of your Node.js application::
In the public_html directory or the directory for your subdomain, create or modify the .htaccess file. Add the following lines to route all requests to your Node.js application:
   Options +FollowSymLinks -Indexes 
   IndexIgnore * 
   DirectoryIndex 
   <IfModule mod_rewrite.c> 
   RewriteEngine on 
   RewriteRule ^(.*)$ http://localhost:<your Port>/\$1 [P] 
   </IfModule>
2-

