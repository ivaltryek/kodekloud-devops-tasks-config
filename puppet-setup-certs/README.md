# Setup Puppet Certs

## Task Description 📔

The Nautilus DevOps team has set up a puppet master and an agent node in Stratos Datacenter. Puppet master is running on jump host itself (also note that Puppet master node is also running as Puppet CA server) and Puppet agent is running on App Server 1. Since it is a fresh set up, the team wants to sign certificates for puppet master as well as puppet agent nodes so that they can proceed with the next steps of set up. You can find more details about the task below:

Puppet server and agent nodes are already have required packages, but you may need to start puppetserver (on master) and puppet service on both nodes.

Assign and sign certificates for both master and agent node.

## Solution

- Edit `/etc/hosts`
  Add puppet alias to jump server entry. For ex:
  ```diff
  -172.16.238.3 jump_host.stratos.xfusioncorp.com jump_host
  +172.16.238.3 jump_host.stratos.xfusioncorp.com jump_host puppet
  ```

- SSH to App server and edit the `/etc/hosts` same way as we did in first step.
  
- Restart the puppet service
  ```bash
  systemctl restart puppet
  systemctl status puppet
  ```

- Sign the certificate from Jump host
  ```bash
  puppetserver ca sign --all
  ```