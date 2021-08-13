# Task Description 📓

The Nautilus development team shared requirements with the DevOps team regarding new application development.—specifically, they want to set up a Git repository for that project. Create a Git repository on Storage server in Stratos DC as per details given below:


Install **git** package using **yum** on Storage **server.**

After that create a **bare** repository **/opt/beta.git** (make sure to use exact name).

---
## Steps

- Login to the Storage Server

  ```console
  thor@jump_host:~$ ssh natasha@ststor01
  The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
  ECDSA key fingerprint is SHA256:n2v4O8tqYwTLr7W82tE544IYclA4YWevycbPXPFcJTs.
  ECDSA key fingerprint is MD5:bd:47:c9:69:e6:90:8d:e5:a3:7b:59:9c:c6:d6:4d:6e.
  Are you sure you want to continue connecting (yes/no)? yes
  Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
  natasha@ststor01's password: Bl@kW

  [natasha@ststor01 ~]$ sudo su -
  We trust you have received the usual lecture from the local System
  Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

  [sudo] password for natasha: Bl@kW

  [root@ststor01 ~]# yum install -y git
  [root@ststor01 ]# cd /opt
  [root@ststor01 opt]# git init --bare beta.git


  ```  

  > Click on Finish task to complete the task.