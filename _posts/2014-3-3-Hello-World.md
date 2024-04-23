layout: post
title: Docker Three Tier Architecture 
---

Next you can update your site name, avatar and other options using the _config.yml file in the root of your repository (shown below).

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
# Three-Tier Application Deployment using Dockerfile

![Architecture](assets/Infra.gif)

This repository demonstrates the deployment of a three-tier application using Docker, focusing on individual Dockerfiles for each component. The application comprises a MySQL database, a Node.js backend, and a React.js frontend.

## Prerequisites

Before you begin, ensure that you have the following installed:

- [Docker](https://www.docker.com/get-started)
  
## Project Structure

- *backend*: Node.js application serving as the backend.
- *frontend*: React.js application for the frontend.
- *mysql*: Dockerfile and configurations for the MySQL database.

## Dockerfile(Database)
![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/1.png)


## Dockerfile(Backend)
![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/2.png)
## Dockerfile(Frontend)
![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/3.png)
## Deployment Steps
0. *Create Network*
   - Navigate to the project directory
   - bash
     docker network create my-network
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/4.png)
1. *MySQL Database:*

   - Navigate to the mysql directory.
   - Build the MySQL Docker image:
     bash
     docker build -t mysql-image .
     
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/5.png)
     
   - Run the MySQL container:
     bash
     docker run --name mysql-container --network=three-tier-network -p 3306:3306 -v mysql-data:/var/lib/mysql -d mysql-image
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/6.png)
   - Access the MySQL container:
     bash
     docker exec -it mysql-container /bin/bash
     
   - Inside the container, create tables for the database:
     sql
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/7.png)
     USE school;
     CREATE TABLE student (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(40), roll_number INT, class VARCHAR(16));
     CREATE TABLE teacher (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(40), subject VARCHAR(40), class VARCHAR(16));
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/8.png)
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/8-1.png)
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/8-2.png)
2. *Backend Application:*

   - Navigate to the backend directory.
   - Build the backend Docker image:
     bash
     docker build -t backend .
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/9.png)
   - Run the backend container:
     bash
     docker run -d -p 3500:3500 --name backend-container --network=three-tier-network backend
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/10.png)
3. *Frontend Application:*

   - Navigate to the frontend directory.
   - Build the frontend Docker image:
     bash
     docker build -t frontend .
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/11.png)
   - Run the frontend container:
     bash
     docker run -d --name frontend-container --network=three-tier-network -p 80:80 frontend
     
     ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/12.png)
4. *Access the Application:*

   Open your favorite browser and visit [http://localhost:80](http://localhost:80). Enjoy exploring the MERN stack application!
   ![Alt Text](https://raw.githubusercontent.com/shaily2106/shaily2106.github.io/master/images/13.png)

    
## Data Persistence

Data persistence is ensured by using Docker volumes. If the MySQL container is deleted, data remains available and is automatically added to a new Docker container by providing the same Docker volume.

Feel free to explore and modify the Dockerfiles to enhance your understanding of containerization and deployment! Happy coding! 🚀
