---
title: configuring a remote repository for a fork
date: 2018-06-05 19:11:54
categories:
  - coding
tags: git
---

You must configure a remote that points to the upstream repository in Git to sync changes 
you make in a fork with the original repository. This also allows you to sync changes made 
in the original repository with the fork.

1. Open Terminal
2. List the current configured remote 
```bash
$ git remote -v
origin https://github.com/YOUR_USERNAME/YOUR_FOCK.git (fetch)
origin https://github.com/YOUR_USERNAME/YOUR_FOCK.git (push)
```
3. Specify a new remote upstream repository that will be synced with the fork
```bash
$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

4. Verify the new upstream repository you've specified for your fork
```bash
$ git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```
5. Fetch the branches and their respective commits from the upstream repository. Commits to **master** will be stored in a local branch, **upstream/master**
```bash
$ git fetch upstream
remote: Counting objects: 75, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 62 (delta 27), reused 44 (delta 9)
Unpacking objects: 100% (62/62), done.
From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
 * [new branch]      master     -> upstream/master
```

6. Check out your fork's local **master** branch
```bash
$ git checkout master
Switched to branch 'master'
```
7. Merge the changes from upstream/master into your local master branch. This brings your fork's master branch into sync with the upstream repository, without losing your local changes
```bash
$ git merge upstream/master
Updating 34e91da..16c56ad
Fast-forward
 README.md                 |    5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)
```

8. Push your changes
```bash
$ git push
```
> [Configuring a remote for a fork](https://help.github.com/articles/configuring-a-remote-for-a-fork/)
> [Syncing a fork](https://help.github.com/articles/syncing-a-fork/)
