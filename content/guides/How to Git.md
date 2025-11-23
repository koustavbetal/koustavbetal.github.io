---
State: Complete
date: '2025-10-04T17:04:33+05:30'
title: How to Git
icon: github
tags:
  - git
  - github
Relations:
Resources: https://education.github.com/git-cheat-sheet-education.pdf
authors: 
  - name: koustav
    link: /members/koustav
    image: https://github.com/koustavbetal.png
---

![](/images/fingerprint-white.svg)
# Install Git
---
```bash
sudo apt install git
```
>*Debian Only*
# Global Config  
---
>***Configuring user information used across all local repositories***
---
### **Set Username**
```bash
git config --global user.name <firstname lastname>
```
>*set a name that is identifiable for credit when review version history*
### **Set User Email**
```bash
git config --global user.email <email>
```
>*set an email address that will be associated with each history marker*


# Initialise Repository
---
>***Configuring user information, initializing, and cloning repositories***
---
### **Initialise a Directory as Repository**
```bash
git init
```
>*Initialize an existing `directory` as a Git repository*
### **Download a Repository _(from GitHub)_**
```bash
git clone <url>
```
>*Retrieve an entire repository from a hosted location through URL*


# Status of Repository
---
>***Theory of staging(add), upstaging(remove) and committing***
>***Staged → Commit  (→ Push)***
---
### **Check the Status:** `status`
```bash
git status 
```
>*Shows any kind of  modifications of files in working directory and whether staged for your next commit.*
### **Stage a file:** `add`
```bash
git add <file_name>
```
>*Adds a Specific file to a staging area*
>*To stage every single changed files or folder to the staged area use dot (.) instead of <file_name> from the working.*
```bash
git add .
```

### **Unstage a file:** `reset`
```bash
git reset <file_name>
```
>*Unstage a file while retaining the changes in working directory*
>Similarly, to unstage everything use dot(.) instead of <file_name>.
>To reset everything from the previous or defined commit use: `--hard` flag.
```bash
git reset --hard <commit>
```

### **Verify the Differences:** `diff`
```bash
git diff
```
>*Verify the difference of what is changed but not staged*
>*And to check the differences of what is changed and also staged but not yet committed add the `--staged` flag:
```bash
git diff --staged
``` 

### **Commit:** `commit`
```bash
git commit -m "<descriptive_message>"
```
>*commit your staged content as a new commit snapshot*
>`-m` flag is used to type the message to determine about the changes afterwards,
>	If we do not use this, another separate window like “nano” will popup to enter a commit message, which increases the complexity, without any benefit.


# Push Repository 
---
>***(Staged → Commit →) push | fetch | pull | merge***
---
### **Add Remote Repository:** `remote add`
```bash
git remote add <alias> <url>
```
>*`alias`: is like the initial branch alias, i.e. ‘origin‘.*
>`url`: is the url of the remote repository i.e. https://github.com/koustavbetal/dotfiles
### **Push a file:** `push`
```bash
git push <alias> <branch>
```
>*`branch`: This is the branch where you want to push the changes.*
>*Transmit local branch commits to the remote repository branch.*
### **Re-base History:** `rebase`
```bash
git rebase <branch>
```
>*It adds or pushes your current changes as the original base of that git branch, It is a risky command in collaborative project yet very  useful to mitigate errors.*
>*To prevent that from happening, always it is recommended to use `fetch`  before doing any kind changes to the files.*
### **Fetching the Recent Changes:** `fetch`
```bash
git fetch <alias>
```
>*\It fetches the recent changes (only the hashes, it is not same as pull) from the remote repository.*
### **Pull the Repository _(from GitHub)_:** `pull`
```bash
git pull
```
>*Fetch and merge any commits from the tracking remote branch.
>No flag is required, as there is only a single repository mapped to the remote.*
### **Merge two Branches:** `commit`
```bash
git merge <alias>/<branch>
```
>*Merge a remote branch into your current branch to bring it up to date.*


