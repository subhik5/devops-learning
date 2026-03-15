# Docker Installation on Ubuntu (EC2)

## What is Docker

Docker is a containerization platform used to package applications with all dependencies and run them anywhere.


## Update Server

sudo apt update


## Install Docker

sudo apt install docker.io -y


## Start Docker

sudo systemctl start docker


## Enable Docker on Boot

sudo systemctl enable docker


## Add User to Docker Group

sudo usermod -aG docker $USER 


## Activate Permission

newgrp docker


## Check Docker Version

docker --version


## Start Docker

sudo systemctl start docker


## Enable Docker on boot

sudo systemctl enable docker


## Start Docker

sudo systemctl start docker


## Test Docker

docker run hello-world
