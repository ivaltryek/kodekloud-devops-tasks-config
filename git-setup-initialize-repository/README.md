# Task Description 📓

## ```The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project. Create a **Git repository on Storage server** in Stratos DC as per details given below:```

## ```Install git package using yum on Storage server. After that create/init a git repository /opt/media.git (use the exact name as asked and make sure not to create a bare repository).```

---

## Solution

```bash
ssh natasha@ststor01
```
> Enter "yes" and after that, enter the password of Storage server: Bl@kW

```bash
sudo su -
```
> With this, you'll be logged in as root and you don't have to add sudo and insert password in every commands.

```bash
yum install -y git
```
> Install the git as it's described in first step.

```bash
cd /opt 
```
> Go to the directory where we need to create git repository

```bash
git init media.git
```
> Initialize the repository with name as it's given in the description.

```bash
ls -la media.git/
```
> Make sure that the repository is initialized.

## ``` Now you can click on finish task.```