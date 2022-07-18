# Managing ACLs Using Ansible

## Task Description 📔

There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

Create a playbook.yml under `/home/thor/ansible` on jump host, an inventory file is already present under `/home/thor/ansible` directory on Jump Server itself.

Create an empty file blog.txt under `/opt/security/` directory on app server 1. Set some acl properties for this file. Using acl provide read '(r)' permissions to group tony (i.e entity is tony and etype is group).

Create an empty file story.txt under `/opt/security/` directory on app server 2. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to user steve (i.e entity is steve and etype is user).

Create an empty file media.txt under `/opt/security/` on app server 3. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to group banner (i.e entity is banner and etype is group).

Note: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way, without passing any extra arguments.

## Solution

- Create `playbook.yml`
  ```yaml
  - name: Create file and set ACL in Host 1
    hosts: stapp01
    become: yes
    tasks:
      - name: Create the blog.txt on stapp01
        file:
          path: /opt/security/blog.txt
          state: touch
      - name: Set ACL for blog.txt
        acl:
          path: /opt/security/blog.txt
          entity: tony
          etype: group
          permissions: r
          state: present

  - name: Create file and set ACL in Host 2
    hosts: stapp02
    become: yes
    tasks:
      - name: Create the story.txt on stapp02
        file:
          path: /opt/security/story.txt
          state: touch
      - name: Set ACL for story.txt
        acl:
          path: /opt/security/story.txt
          entity: steve
          etype: user
          permissions: rw
          state: present

  - name: Create file and set ACL in Host 3
    hosts: stapp03
    become: yes
    tasks:
      - name: Create the media.txt on stapp03
        file:
          path: /opt/security/media.txt
          state: touch
      - name: Set ACL for media.txt
        acl:
          path: /opt/security/media.txt
          entity: banner
          etype: group
          permissions: rw
          state: present
  ```

- Run the playbook
  ```bash
  ansible-playbook -i inventory playbook.yml
  ```

- Verify the changes
  ```bash
  ansible all -a "ls -ltr /opt/security/" -i inventory
  ```