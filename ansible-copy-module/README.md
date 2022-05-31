# Ansible Copy Module

## Task Description 📔

There is `data` on `jump host` that needs to be `copied` on `all application servers` in Stratos DC. Nautilus DevOps team want to perform this task using Ansible. Perform the task as per details mentioned below:

a. On jump host create an `inventory` file `/home/thor/ansible/inventory` and add all application servers as managed nodes.

b. On jump host create a `playbook` `/home/thor/ansible/playbook.yml` to copy `/usr/src/data/index.html` file to all application servers at location `/opt/data`.

Note: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra argument

## Solution

- Create Inventory file
  ```
  stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n  ansible_user=tony
  stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@  ansible_user=steve
  stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n  ansible_user=banner
  ```

- Create Playbook file
  
  ```yaml
  - name: Ansible copy
    hosts: all
    become: yes
    tasks:
      - name: copy index.html to data folder
        copy: src=/usr/src/data/index.html dest=/opt/data
  ```

- Run the playbook
  
  ```bash
  ansible-playbook -i inventory playbook.yml
  ```

- Check if the files are copied
  
  ```bash
  ansible all -a "ls -ltr /opt/sysops" -i inventory
  ```