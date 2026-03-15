# Docker Compose

## What is Docker Compose

Docker Compose is used to run multiple containers using a single YAML file.
Example:
Web server + Database running together.


## Install Docker Compose
sudo apt install docker-compose -y


## Check Version
docker compose version


## Create docker-compose.yml
Example

version: "3"

services:

  web:
    image: nginx
    ports:
      - "80:80"
 

## Run Containers
docker compose up -d

This command will
1 Pull image if not present
2 Create container
3 Start container


## Stop Containers
docker compose down 
 

## View Running Containers
docker compose ps



## Real DevOps Workflow (after cloning repo)

Suppose you clone a project.

git clone project-repo
cd project-repo

You may see:
Dockerfile
docker-compose.yml

Now you run:
docker compose up -d

Docker Compose will:


read docker-compose.yml
↓
build image (if build defined)
↓
create containers
↓
connect network
↓
create volumes
↓
start application

Everything automatically.




##  Example Real Compose File

version: "3"

services:

  web:
    build: .
    ports:
      - "5000:5000"

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:

When you run:
docker compose up -d

Docker will:

-build web image
-create web container
-pull mysql image
-create mysql container
-create volume mysql-data
-connect both containers
-start everything

##  Why DevOps Engineers Love Docker Compose

Because instead of running many commands like:

docker run
docker network create
docker volume create

You just run one command:

docker compose up -d

Everything happens automatically.















