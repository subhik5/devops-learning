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






# Connecting EC2 to GitHub Using Git and Personal Access Token

This guide explains how to connect an EC2 instance to GitHub so that we can push code from the server.



## Step 1: Install Git

sudo apt update
sudo apt install git -y

Check version

git --version



## Step 2: Configure Git

Set username:-

git config --global user.name "Your GitHub Username"

Set email

git config --global user.email "your-email@example.com"

Check configuration

git config --list

---

## Step 3: Create a Repository on GitHub

Go to GitHub and create a new repository.

Example repository name:

devops-learning

Do not initialize it with README if you already have files locally.



## Step 4: Initialize Git Repository

Navigate to your project folder.:-
cd devops-learning

Initialize git:-
git init



## Step 5: Add Files

git add .

Check status

git status


## Step 6: Commit Files

git commit -m "Initial commit for devops learning repository"



## Step 7: Add Remote Repository

git remote add origin https://github.com/YOUR_USERNAME/devops-learning.git

Verify remote
git remote -v


## Step 8: Create Personal Access Token

Go to GitHub.

Settings
Developer Settings
Personal Access Tokens
Tokens (Classic)

Generate new token.

Select permissions:

repo

Copy the token.

---

## Step 9: Push Code to GitHub

git branch -M main

git push -u origin main

Git will ask:

Username → GitHub username  
Password → Personal Access Token

Paste the token when asked for password.


## Step 10: Verify on GitHub

Open the repository in GitHub.
You should see your files uploaded successfully.
