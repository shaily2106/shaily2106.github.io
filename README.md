# Docker 3 Tier Architecture Dockerfile Tutorial


*Backend Dockerfile*

 - Use the official Node.js image as a base image
 bash 
 FROM node:14
 
 - Set the working directory inside the container
 bash
 WORKDIR /app
 
 - Copy package.json and package-lock.json to the working directory
 bash
 COPY package*.json ./
 
 - Install dependencies
 bash
 RUN npm install
 
 - Copy the rest of the application code to the working directory
 bash 
 COPY . .
 
 - Expose the port that your app is running on
 bash
 EXPOSE 3500
  
 - Command to run your application
 bash
 CMD ["node", "server.js"]
 

*Database Dockerfile*

 bash
 FROM mysql:latest
 
 - Set environment variables
 bash
 ENV MYSQL_ROOT_PASSWORD=mysql123
 ENV MYSQL_DATABASE=school
 
 - Expose the MySQL port
 bash
 EXPOSE 3308
 
 - Define a named volume for MySQL data
 bash
 VOLUME /var/lib/mysql
 
 - Command to run the MySQL server
 bash
 CMD ["mysqld"]
 


*Frontend Dockerfile*

 - Run latest node 
 
     bash
     FROM node:latest as build 
     
  
 - Set the working 
      bash 
     WORKDIR /app 
      
   
 - Run all npm command and copy this to working Directory if not 
     bash 
        COPY . .
        RUN npm install 
     
 - Run npm run build for building and runnnig frontend
     bash 
       RUN npm run build
     

 - Use Nginx to serve the frontend content
     bash 
     FROM nginx:latest
     COPY --from=build /app/build /usr/share/nginx/html
     EXPOSE 80
     CMD ["nginx", "-g", "daemon off;"]

     
