# Task Description 📓

Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:


a. Pull **busybox:musl** image on **App Server 1** in Stratos DC and **re-tag** (create new tag) this image as **busybox:local.**

## Solution

```console
thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:YAahgDLgKj/q1J4aWpr6v2SULniAT5iWwgDpj/0zGbs.
ECDSA key fingerprint is MD5:d0:ac:f3:34:7c:6e:80:c2:f9:a1:26:0f:dd:33:99:32.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.
tony@stapp01's password: 

[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 

[root@stapp01 ~]# docker pull busybox:musl
musl: Pulling from library/busybox
0a4a0dc5db2c: Pull complete 
Digest: sha256:3231ef2e0d1f8f91f9d3fed494f64cd325798ba417f679d9ea4803ce46f6598e
Status: Downloaded newer image for busybox:musl
docker.io/library/busybox:musl

[root@stapp01 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
busybox      musl      33062ebf2504   2 days ago   1.43MB

[root@stapp01 ~]# docker image tag busybox:musl busybox:local

[root@stapp01 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
busybox      local     33062ebf2504   2 days ago   1.43MB
busybox      musl      33062ebf2504   2 days ago   1.43MB
```

Click on **submit** to finish the task ✔️