<<In Docker, volumes are used to store data outside the container so that data is not lost when the container stops or is deleted. Think of it like a USB drive attached to a computer — even if the computer shuts down, the USB still keeps the files. 💾

I’ll explain all important commands step-by-step in a simple DevOps way.>>



## 1️⃣ Create a Docker Volume

 Command:

 docker volume create myvolume

# Example:

 docker volume create nginx-data

 This creates a volume named nginx-data.

## Check volumes:

 docker volume ls

 Output example

 DRIVER    VOLUME NAME
 local     nginx-data



## 2️⃣ Inspect a Volume (see where it is stored)

 docker volume inspect nginx-data

 Example output

 Mountpoint: /var/lib/docker/volumes/nginx-data/_data

 This is where Docker stores the data on your system.



## 3️⃣ Run Container and Mount Volume

Syntax:

 docker run -v volume-name:container-path image

 Example with nginx

 docker run -d -p 80:80 -v nginx-data:/usr/share/nginx/html nginx

 Explanation:

 Part Meaning

 nginx-data	Docker volume
 /usr/share/nginx/html	Path inside container
 nginx	image


 So:

 Volume → nginx-data
 Container folder → /usr/share/nginx/html

 Any files inside this folder are stored in the volume.



## 4️⃣ Create Volume Automatically During Run

 You do not need to create it manually.

 docker run -d -v mydata:/data nginx

 Docker will automatically create mydata volume.



## 5️⃣ Bind Mount (Mount Local Folder Instead of Volume)

 Sometimes you want to mount a host directory.

 Syntax

 docker run -v /host/path:/container/path image

 Example

 docker run -d -p 80:80 -v /home/komal/site:/usr/share/nginx/html nginx

 Meaning

 Host folder → /home/komal/site
 Container folder → /usr/share/nginx/html

 So if you change files on host → container reflects immediately.




## 6️⃣ Use --mount (Modern Method)

 This is the recommended method.

 Syntax

 docker run --mount source=volume-name,target=/container/path image

 Example

 docker run -d \
 --mount source=nginx-data,target=/usr/share/nginx/html \
 -p 80:80 nginx



## 7️⃣ Mount Read-Only Volume

 Sometimes containers should not modify data.

 docker run -v nginx-data:/data:ro nginx

 ro = read only



## 8️⃣ Mount Volume to Multiple Containers

 Same volume can be used by many containers.

 Container 1

 docker run -d -v shared-data:/data nginx

 Container 2

 docker run -d -v shared-data:/data ubuntu

 Both containers share same data.



## 9️⃣ Copy Files into Volume (using container)

 docker run --rm -v nginx-data:/data ubuntu touch /data/file1.txt

 Now the file exists inside the volume.
 
 

## 🔟 Remove Volume

 Delete a volume

 docker volume rm nginx-data

 Remove all unused volumes

 docker volume prune



## 1️⃣1️⃣ Remove Container but Keep Volume

 docker rm container_name

 Volume still exists.

 Data is safe.



## 1️⃣2️⃣ Check Which Container Uses Volume

 docker inspect container_name

Look for Mounts section.



💡 Most Important Commands (DevOps Quick List)

docker volume create volume_name
docker volume ls
docker volume inspect volume_name
docker volume rm volume_name
docker volume prune

docker run -v volume:container_path image
docker run -v host_path:container_path image
docker run --mount source=volume,target=container_path image


---

🎯 Real DevOps Example

Run Nginx with persistent website files

docker volume create webdata

docker run -d -p 80:80 \
-v webdata:/usr/share/nginx/html \
nginx

Now even if container is deleted:

docker rm -f container

Your website files remain in webdata volume.



## ⭐ Pro DevOps Tip

Inside Docker host:

/var/lib/docker/volumes/

All volumes live here.

Great question. This is exactly how DevOps engineers learn Docker in real projects. We'll go step-by-step like a lab. Imagine we are setting up a database server container using MySQL.

I’ll answer your two concepts first, then we will do hands-on steps.


## TOPIC=2

## MYSQL volume concept
## 1️⃣ Why we use -e MYSQL_ROOT_PASSWORD=root

<<When you run a MySQL container, MySQL must have a root password to start.

The official MySQL Docker image checks for environment variables during startup.

So we pass variables using -e.>>

Example:

docker run -d \
-e MYSQL_ROOT_PASSWORD=root \mysql


Explanation

Part Meaning

-e= environment variable
MYSQL_ROOT_PASSWORD	=variable name expected by MySQL image
root =password value


Inside the container, MySQL reads it like:

ROOT PASSWORD = root

So environment variables are used to configure containers without modifying the image.

This is very common in Docker.

Example variables for MySQL:

MYSQL_ROOT_PASSWORD
MYSQL_DATABASE
MYSQL_USER
MYSQL_PASSWORD

Example:

docker run -d \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=mydb \
-e MYSQL_USER=dev \
-e MYSQL_PASSWORD=dev123 \
mysql

This will create:

database → mydb
user → dev
password → dev123



# 2️⃣ Why volume path is /var/lib/mysql

Every container image has a default data directory.

For the MySQL image:
/var/lib/mysql 

This is where MySQL stores:
databases
tables
logs

So if the container dies and no volume exists, data disappears.

That is why we mount a volume.

Example:

docker run -d \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql

Meaning:

docker volume → mysql-data
container path → /var/lib/mysql

Now your database files are stored in the volume.




# ❗ Important Concept

The container path depends on the software inside the image, not Docker.

Examples:

Application	Data directory

MySQL	= /var/lib/mysql
PostgreSQL =	/var/lib/postgresql/data
MongoDB =	/data/db
Nginx =	/usr/share/nginx/html


So container paths are different for each image.



# 3️⃣ Real DevOps Lab — MySQL with Volume

Step 1 — Create Volume
docker volume create mysql-data

Check:
docker volume ls



Step 2 — Run MySQL Container

docker run -d \
--name mysql-container \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-p 3306:3306 \
mysql

Explanation:

Option	Meaning

--name mysql-container =	container name
-v mysql-data:/var/lib/mysql	= volume mapping
-e MYSQL_ROOT_PASSWORD=root	root password
-p = 3306:3306	database port
mysql	= image




Step 3 — Check Running Container

docker ps



Step 4 — Enter MySQL Container

docker exec -it mysql-container bash

Then run:

mysql -u root -p
Password:
root

Now you are inside MySQL.



Step 5 — Create Database

Inside MySQL:
CREATE DATABASE testdb;

Check:
SHOW DATABASES;



Step 6 — Stop Container

Exit container:
exit

Stop container:
docker stop mysql-container



Step 7 — Delete Container

docker rm mysql-container
Now normally data should be gone.
But because we used volume, data is still safe.


Step 8 — Run New Container Using Same Volume

docker run -d \
--name mysql-new \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-p 3306:3306 \
mysql

Now enter again:
docker exec -it mysql-new mysql -u root -p

Check:
SHOW DATABASES;

You will still see:
testdb

💥 This proves volume saved your data.


## 🧠 DevOps Mental Model

Think like this:
Container = Temporary machine
Volume = Hard disk

Even if machine dies, disk survives.


<<TOPIC -3
 NGINX and VolumE container >>

## 1️⃣ Scenario 1 — Deploy a Website using Nginx + Volume

We will run a web server using Nginx and store website files in a Docker volume.
Think of it like this:

Browser → Nginx Container → Volume → Website Files


# Step 1 — Create a Volume

docker volume create webdata
Check:
docker volume ls


# Step 2 — Run Nginx Container with Volume

docker run -d \
--name nginx-server \
-p 80:80 \
-v webdata:/usr/share/nginx/html \
nginx

Explanation:
Part Meaning

webdata	Docker volume =
/usr/share/nginx/html = Website directory inside container


In Nginx this path stores HTML files.


# Step 3 — Enter Container

docker exec -it nginx-server bash

Go to website directory:
cd /usr/share/nginx/html

Create a simple website:
echo "Hello DevOps World" > index.html

Exit container:
exit


# Step 4 — Open Browser

Go to:
http://localhost

You will see:
Hello DevOps World


# Step 5 — Delete Container

docker rm -f nginx-server

Container is gone.
But the volume still exists.


# Step 6 — Run New Container with Same Volume

docker run -d \
--name nginx-new \
-p 80:80 \
-v webdata:/usr/share/nginx/html \
nginx

Open browser again.
Your website is still there.

💡 Because data lives inside volume, not container.


# 2️⃣ Scenario 2 — App + Database using Docker Compose

Now imagine a real application:
Application → Database

We will use:
Docker Compose

MySQL

Volume for database storage


# Step 1 — Create Project Folder

mkdir docker-project
cd docker-project



# Step 2 — Create Compose File

Create:
docker-compose.yml

Example:

version: '3'

services:

  mysql:
    image: mysql
    container_name: mysql-db
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  mysql-data:



# Step 3 — Start Application

Run:
docker compose up -d

This will automatically:
create container
create volume
start MySQL


# Step 4 — Check Containers

docker ps


# Step 5 — Check Volumes

docker volume ls

You will see:
docker-project_mysql-data


# Step 6 — Stop Everything

docker compose down

Containers stop.
But volume remains.
So database data is safe.


# 🧠 Important DevOps Concept

Containers should be stateless.
Container = application
Volume = data

Example production architecture:

Nginx container
      │
Application container
      │
MySQL container
      │
Docker volume


## ⭐ One More Powerful Concept

You will often see this pattern in real DevOps:

docker run -d \
-e MYSQL_ROOT_PASSWORD=root \
-v mysql-data:/var/lib/mysql \
mysql

Why?

-e → configuration
-v → data persistence

Together they make containers production-ready.


## TOPIC = 4

## 1️⃣ Volume vs Bind Mount

Both are used to store data outside the container.
But they work differently.

# 2️⃣ Docker Volume (Docker controls storage)

Example:

docker run -d \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql

Here:

mysql-data → Docker volume
/var/lib/mysql → container path

Docker stores volume here:
/var/lib/docker/volumes/mysql-data/_data

You do not manage this path manually.
Docker manages it.
Think of it like:
Container → Docker volume → Docker storage
 

# 3️⃣ Bind Mount (You control the folder)

Instead of volume name, we give host directory.
Example:

docker run -d \
-v /home/user/website:/usr/share/nginx/html \
nginx

Here:
Host folder → /home/user/website
Container folder → /usr/share/nginx/html

So files are stored directly in your system.
If you edit files:
/home/user/website/index.html

Container instantly sees changes.
Think like:
Container → Host folder



4️⃣ Real Life Example

Bind Mount (used by developers)
Developer edits code locally.
docker run -v $(pwd):/app node
Local code automatically reflects inside container.

Docker Volume (used in production)
Database storage.
docker run -v mysql-data:/var/lib/mysql mysql
Because database data must be safe and managed.



🧠 Easy Way To Remember

Bind Mount = Your folder
Volume = Docker folder
