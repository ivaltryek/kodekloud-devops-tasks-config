# Task Description 📓

The Nautilus development team started with new project development. They have created different Git repositories to manage respective project's source code. One of the repo **/opt/news.git** was created recently. The team has given us a sample **index.html** file that is currently present on jump host under **/tmp**. The repository has been cloned at **/usr/src/kodekloudrepos** on storage server in Stratos DC.



Copy sample index.html file from jump host to storage server under cloned repository at **/usr/src/kodekloudrepos**, add/commit the file and push to **master** branch.

---

## Steps

- Copy **index.html** to Storage Server's tmp folder, we don't have the permission to copy it directly to the required destination folder.

  ```bash
  sudo scp /tmp/index.html natasha@ststor01:/tmp/
  ``` 
  > It'll prompt to enter the thor's password **mjolnir123** and after that you need to enter Storage Server's password **Bl@kW**.

- ssh into the Storage Backup server.

  ```bash
  ssh natasha@ststor01
  sudo su -
  ``` 
  > Enter the Storage Server's password **Bl@kW**.

- Copy index.html to the destination folder.

  ```bash
  cp /tmp/index.html /usr/src/kodekloudrepos/news/
  ``` 

- Now check if the changes are done.

  ```bash
  cd /usr/src/kodekloudrepos/news/
  ls 
  ``` 
  > Make sure that **index.html** is there.

- Now commit the changes to git.

  ```shell
  git add .
  git commit -m "Added index.html"
  git push origin master
  ``` 

Click on **Submit** button to finish the task.