# Docker Volume Mapping

# Task Description 📔

The Nautilus DevOps team is testing applications containerization, which issupposed to be migrated on docker container-based environments soon. In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:

a. On `App Server 3` in Stratos DC pull `nginx` image (preferably `latest` tag but others should work too).

b. Create a new container with name `ecommerce` from the image you just pulled.

c. Map the host volume `/opt/devops` with container volume `/tmp`. There is an `sample.txt` file present on same server under `/tmp`; copy that file to `/opt/devops`. Also please keep the container in `running` state

## Solution

- SSH into the App server
- Pull the image
  ```bash
  docker pull nginx:latest
  ```
- Copy the txt file to the directory.
  ```bash
  cp /tmp/sample.txt /opt/devops
  ```
- Run the Container
  ```bash
  docker run --rm -d -v /opt/devops:/tmp --name ecommerce nginx:latest
  ```
- Check if everythings good
  ```bash
  docker exec <container-id> cat /tmp/sample.txt
  ```

> **Note** <br>
If it return the output then click on complete to finish.