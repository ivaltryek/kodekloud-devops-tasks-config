# Puppet Add Users

## Task Description 📔

A new teammate has joined the Nautilus application development team, the application development team has asked the DevOps team to create a new user account for the new teammate on application server 2 in Stratos Datacenter. The task needs to be performed using Puppet only. You can find more details below about the task.

Create a Puppet programming file `blog.pp` under `/etc/puppetlabs/code/environments/production/manifests` directory on master node i.e Jump Server, and using Puppet user resource add a user on all app servers as mentioned below:

Create a user `yousuf` and set its UID to `1745` on Puppet agent nodes 2 i.e `App Servers 2`.

Notes: :- Please make sure to run the puppet agent test using sudo on agent nodes, otherwise you can face certificate issues. In that case you will have to clean the certificates first and then you will be able to run the puppet agent test.

:- Before clicking on the Check button please make sure to verify puppet server and puppet agent services are up and running on the respective servers, also please make sure to run puppet agent test to apply/test the changes manually first.

:- Please note that once lab is loaded, the puppet server service should start automatically on puppet master server, however it can take upto 2-3 minutes to start.

## Solution

- Create `blog.pp`
  ```ruby
  class user_creator {
    user { 'yousuf':
    ensure   => present,
    uid => 1745,

    }
  }

  node 'stapp02.stratos.xfusioncorp.com' {
    include user_creator
  }

  ```

- Validate the file
  ```bash
  puppet parser validate /etc/puppetlabs/code/environments/production/manifests/blog.pp
  ```

- SSH into the required servers where user needs to be created, App server 2 in this case.
- Run the puppet command
  ```bash
  puppet agent -tv
  ```

- Check if user is created.
  ```bash
  cat /etc/passwd | grep yousuf
  ```