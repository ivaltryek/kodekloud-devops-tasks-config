# Docker Ports Mapping

## Task Description 📔

The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 1 in Stratos Datacenter. Please perform the task as per details mentioned below:

a. Pull nginx:stable docker image on Application Server 1.

b. Create a container named media using the image you pulled.

c. Map host port 5004 to container port 80. Please keep the container in running state.

## Solution

- SSH into the App server
  ```bash
  ssh tony@stapp01
  sudo su
  ```

- Pull the docker image
  ```bash
  docker pull nginx:stable
  ```

- Start the container
  ```bash
  docker run -d --name media -p 5004:80 nginx:stable
  ```