# Task Description 📓
Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on **Application Server 2**. Please complete the task as per details given below:

On **Application Server 2** create a **container** named **nginx_2** using **image** **nginx** with **alpine** tag and make sure container is in **running** state.

---
## Solution

```console
thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:v1XUTYfKURmxBKV+CTFmdS+9tRK4G86wXukRQhfa+so.
ECDSA key fingerprint is MD5:1c:9b:58:94:82:53:b3:4b:23:ec:c1:06:d5:0d:e5:ab.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: 
[steve@stapp02 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 

[root@stapp02 ~]# docker run -d --name nginx_2 -p 8080:80 nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
29291e31a76a: Pull complete 
e82f830de071: Pull complete 
d7c9fa7589ae: Pull complete 
3c1eaf69ff49: Pull complete 
bf2b3ee132db: Pull complete 
9a6ac07b84eb: Pull complete 
Digest: sha256:bead42240255ae1485653a956ef41c9e458eb077fcb6dc664cbc3aa9701a05ce
Status: Downloaded newer image for nginx:alpine
89f9712129f26aaccdcee7899165c5a36b2988f41d13aea5a788dfcf719835ef

[root@stapp02 ~]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
89f9712129f2   nginx:alpine   "/docker-entrypoint.…"   19 seconds ago   Up 16 seconds   0.0.0.0:8080->80/tcp   nginx_2

[root@stapp02 ~]# curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

Click on **submit** to finish the task ✔️