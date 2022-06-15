# Git Revert Some Changes

## Task Description 📔

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/ecommerce` present on` Storage server` in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo `HEAD` to `last` commit. Below are more details about the task:

In `/usr/src/kodekloudrepos/ecommerce` git repository, `revert` the `latest` commit ( HEAD ) to the `previous` commit (JFYI the previous commit hash should be with initial commit message ).

Use `revert ecommerce` message (please use all small letters for commit message) for the new revert commit.

## Solution

- ssh into the storage server
  
  ```bash
  ssh natasha@ststor01
  ```

- revert the changes
  
  ```bash
  git log master
  git revert HEAD
  git add .
  git commit -m "revert ecommerce"
  git log master
  ```