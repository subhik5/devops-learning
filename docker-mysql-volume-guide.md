Docker MySQL Volume and Environment Variables Guide

1. Overview

This guide explains how to run a MySQL container using Docker with volumes and environment variables.
It is useful for beginners learning Docker and DevOps practices.

Docker containers are temporary. When a container is removed, all data inside it disappears unless we store the data in a Docker volume.



2. Why We Use Environment Variables (-e)

Environment variables allow us to configure a container when it starts without modifying the Docker image.
Example:

docker run -d -e MYSQL_ROOT_PASSWORD=root mysql

This command:
- Starts a MySQL container
- Sets the root password to root



3. Minimum Required Variable

The official MySQL Docker image requires only one mandatory variable.

-e MYSQL_ROOT_PASSWORD=root

Example:

docker run -d \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql

This will:

- Start MySQL
- Set the root password
- No database or extra user will be created

You can create databases manually later.
 


4. Creating Database and User Automatically

You can also pass additional environment variables so the container automatically creates a database and user when it starts.
Example:

docker run -d \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=mydb \
-e MYSQL_USER=dev \
-e MYSQL_PASSWORD=dev123 \
mysql

This automatically creates:

Database: mydb
User: dev
Password: dev123



5. Meaning of Each Environment Variable :-
MYSQL_ROOT_PASSWORD
Sets the admin password for MySQL root user:-

MYSQL_DATABASE
Creates a database when the container starts.

MYSQL_USER
Creates a new database user.

MYSQL_PASSWORD
Password for the new user.

Important rule:
MYSQL_USER must always be used together with MYSQL_PASSWORD



6. Why Applications Should Not Use Root User

Bad practice:
Application connects using root user.

Good practice:
Create a dedicated user for the application.

Example:
Database: ecommerce
User: appuser
Password: app123

This improves security and prevents accidental damage to the database.



7. Why We Use Docker Volumes

Containers are temporary. If a container is deleted, all its data is lost.
To persist data we attach a Docker volume.
Create volume:

docker volume create mysql-data

Run container with volume:-

docker run -d \
--name mysql-container \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-p 3306:3306 \
mysql

Volume mapping:

mysql-data -> /var/lib/mysql

This folder is where MySQL stores:
- databases
- tables
- logs



8. Data Persistence Test

Step 1 — Create database inside MySQL

CREATE DATABASE testdb;

Step 2 — Stop container

docker stop mysql-container

Step 3 — Remove container

docker rm mysql-container

Step 4 — Run container again using same volume

docker run -d \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql

Result:

Database testdb still exists because it was stored in the volume.



9. Common DevOps MySQL Docker Command

docker run -d \
--name mysql-db \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=myapp \
-e MYSQL_USER=myappuser \
-e MYSQL_PASSWORD=myapppass \
-p 3306:3306 \
mysql

This command is commonly used in development environments.


10. Summary

- "MYSQL_ROOT_PASSWORD" is mandatory.
- Other environment variables are optional but useful for automation.
- Volumes store database data outside the container.
- Using volumes ensures data is not lost when containers are removed.

This pattern is widely used in Docker-based development and DevOps environments.
