# Git clone repositories

## Task Description 📔

DevOps team created a new Git repository last week; however, as of now no team is using it. The Nautilus application development team recently asked for a copy of that repo on Storage server in Stratos DC. Please clone the repo as per details shared below:

The repo that needs to be cloned is `/opt/media.git`

Clone this git repository under `/usr/src/kodekloudrepos` directory. Please do not try to make any changes in repo.


## Solution

- SSH into storage server
  ```bash
  ssh natasha@ststor01
  sudo su
  ```

- `cd` into `/usr/src/kodekloudrepos` and clone the repo
  ```bash
  cd /usr/src/kodekloudrepos
  git clone /opt/media.git
  ```