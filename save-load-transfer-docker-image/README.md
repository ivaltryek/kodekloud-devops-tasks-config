# Task Description 📓

One of the DevOps team members was working on to create a new custom docker image on **App Server 1** in Stratos DC. He is done with his changes and image is saved on same server with name **apps:datacenter**. Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on **App Server 3**. So we need to **provide** them that image on **App Server 3** in Stratos DC.

a. On **App Server 1** save the image apps:datacenter in an archive.

b. **Transfer** the image archive to **App Server 3**.

c. **Load** that image archive on **App Server 3** with same name and tag which was used on App Server 1.

Note: Docker is already installed on both servers; however, if its service is down please make sure to **start** it.

---
## Solution

```console
thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:KkjvpPYYAfUj67D8dszgPtm5o89bRmKdqsm7bO+LQ9k.
ECDSA key fingerprint is MD5:5e:27:09:cf:63:b5:60:75:68:25:d1:01:ed:f9:a0:3a.
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
[root@stapp01 ~]# docker images
REPOSITORY   TAG          IMAGE ID       CREATED          SIZE
apps         datacenter   bc06a25872de   52 seconds ago   103MB
ubuntu       latest       1318b700e415   4 weeks ago      72.8MB

[root@stapp01 ~]# docker save -o /tmp/apps.tar apps:datacenter
[root@stapp01 ~]# scp /tmp/apps.tar banner@stapp03:/tmp/
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:6JTqTajmbwsH8NJO3KcwoW0lYeUvsFmydRfB9aQWnWY.
ECDSA key fingerprint is MD5:d8:f3:13:a2:a5:65:0e:c7:4f:6e:0a:97:bd:56:cd:3a.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: 
apps.tar                                                                     100%  100MB  33.0MB/s   00:03 

thor@jump_host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:6JTqTajmbwsH8NJO3KcwoW0lYeUvsFmydRfB9aQWnWY.
ECDSA key fingerprint is MD5:d8:f3:13:a2:a5:65:0e:c7:4f:6e:0a:97:bd:56:cd:3a.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: 
[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[root@stapp03 ~]# cd /tmp/
[root@stapp03 tmp]# ls
apps.tar  ks-script-rnBCJB  yum.log
[root@stapp03 tmp]# docker load -i apps.tar 
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
[root@stapp03 tmp]# systemctl start docker
[root@stapp03 tmp]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2021-08-25 17:21:12 UTC; 13s ago
     Docs: https://docs.docker.com
 Main PID: 657 (dockerd)
    Tasks: 20
   Memory: 143.7M
   CGroup: /docker/2d447c81fc5425b56946a96af7a0c8ae9124e8857431dbbb1cfe435cd6d99382/system.slice/docker.service
           └─657 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Aug 25 17:21:11 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:11.752169168Z" level=...t"
Aug 25 17:21:11 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:11.753266729Z" level=...r"
Aug 25 17:21:11 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:11.753296393Z" level=...t"
Aug 25 17:21:11 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:11.753303507Z" level=...e"
Aug 25 17:21:11 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:11.755487741Z" level=...."
Aug 25 17:21:12 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:12.260863063Z" level=...."
Aug 25 17:21:12 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:12.502470033Z" level=....7
Aug 25 17:21:12 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:12.503375685Z" level=...n"
Aug 25 17:21:12 stapp03.stratos.xfusioncorp.com systemd[1]: Started Docker Application Container Engine.
Aug 25 17:21:12 stapp03.stratos.xfusioncorp.com dockerd[657]: time="2021-08-25T17:21:12.802837380Z" level=...k"
Hint: Some lines were ellipsized, use -l to show in full.


[root@stapp03 tmp]# docker load -i apps.tar 
7555a8182c42: Loading layer [==================================================>]  75.16MB/75.16MB
cc9cb8c4aa1e: Loading layer [==================================================>]  30.11MB/30.11MB
Loaded image: apps:datacenter
[root@stapp03 tmp]# docker images
REPOSITORY   TAG          IMAGE ID       CREATED         SIZE
apps         datacenter   bc06a25872de   5 minutes ago   103MB
```