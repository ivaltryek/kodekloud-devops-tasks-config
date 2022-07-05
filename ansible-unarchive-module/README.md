# Ansible Unarchive Module

## Task Description 📔

Extract archive stored at `/usr/src/devops/xfusion.zip` in Jump host to all the app servers at location `/opt/devops/`. The user should be the owner of the app server. And mode should be `0744`

## Solution

- Create playbook.yml
  ```yaml
  - name: Extract archive
    hosts: stapp01, stapp02, stapp03
    become: yes
    tasks:
      - name: Extract the archive and set the owner/permissions
        unarchive:
          src: /usr/src/devops/xfusion.zip
          dest: /opt/devops/
          owner: "{{ ansible_user }}"
          group: "{{ ansible_user }}"
          mode: "0744"
  ```

- Apply the playbook
  ```bash
  ansible-playbook -i inventory playbook.yml
  ```

- Verify the changes
  ```bash
  ansible all -a "ls -ltr /opt/devops/" -i inventory
  ```