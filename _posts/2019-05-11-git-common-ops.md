---
layout: posts
title:  "Common operations"
categories: [git]
tags: [git]
---

My team follow this for day to day usage.

## Make a new branch

### Branch Naming convention

According to what you working on, you need to create a new branch to work on.
This branch name MUST be in lower case, and start with the correct prefix 
according to its content.

- Hot fix MUST start with the prefix `hotfix-`
- New feature MUST start with the prefix `feature-`  
- Bug fix MUST start with the prefix `fix-{issue_number}-`  

> Every bug fix MUST have an associated issue.

### Branch origin

When your create a branch, it always start from the last commit of the current branch.
So your branch may be created from `develop` or `master` on this project.

- Branch create from `develop` can be merge in `develop` only. 
- Branch create from `master` can be merge in `master` or `develop`.

So:

- **Hot fix branch** are always created from `master`.
- **bug fix branch** are always created from `develop`.
- **Feature branch** may be create from `develop` or `master`, according to which 
merging strategy need to be apply.

### Commands

```bash
    # go on master or develop
    git checkout master|develop
    
    # update current branch
    git pull
    
    # create local branch
    git checkout -b branch_name
    
    # create remote branch
    git push --set-upstream origin branch_name
```

## Remove a branch

```bash
    # remove local branch
    git branch -D branch_name
    
    # remove remote branch
    git push origin :branch_name
```

## Edit the last commit

Add current index to the last commit and open commit message editor

```bash
    git commit --amand
    # or with gitconfig alias
    git amand
```

Add current index to the last commit and keep old message

```bash
    git commit --amand --no-edit
    # or with gitconfig alias
    git oops
```

## Play with stash

The git stash command takes your uncommitted changes (both staged and unstaged) and 
saves them away for later use.

By default, running git stash will stash:

- changes that have been added to your index (staged changes)
- changes made to files that are currently tracked by Git (unstaged changes)

But it will not stash:

- new files in your working copy that have not yet been staged
- files that have been ignored

Use `git add` for them.

You aren't limited to a single stash. You can run git stash several times to
create multiple stashes,

and then use `git stash list` to view them.

    $ git stash list
    stash@{0}: On master: stash_message
    stash@{1}: On develop: stash_message
    
- Restore a stash using `git stash apply stash@{INDEX}`.
- Remove a stash using `git stash clear stash@{INDEX}`.
 
Using `git stash pop stash@{INDEX}` is equivalent to `git stash apply stash@{INDEX} && git stash clear stash@{INDEX}`.

[more info](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

## Return to a previous commit

Given the this commit history

    * caf0816 - (HEAD -> feature) last commit (John il y a 16 minutes)
    * 71a28c3 - (origin/feature) commit two (John il y a 3 jours)
    * 79bb856 - commit one (John il y a 3 jours)

If you want to go back to `79bb856`, keeping modification of commits `71a28c3` and `caf0816`

```bash
    git reset 79bb856
```

If you want to go back to the exact state of `79bb856`

```bash
    git reset --hard 79bb856
```

## Rebase

Rebasing is the process of moving or combining a sequence of commits to a new base commit.
Rebasing is most useful and easily visualized in the context of a feature branching workflow. 

The general process can be visualized as the following:
![rebase](https://wac-cdn.atlassian.com/dam/jcr:e4a40899-636b-4988-9774-eaa8a440575b/02.svg?cdnVersion=kx)

From a content perspective, rebasing is changing the base of your branch from one 
commit to another making it appear as if you'd created your branch from a different commit.

Internally, Git accomplishes this by creating new commits and applying them to the 
specified base. It's very important to understand that even though the branch looks 
the same, it's composed of entirely new commits.

For better understanding please read [this article](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase).

### commands

This operation will rewrite the graph commit history, please only use it when
you make a merge request which as conflict.

```bash
    # my merge request from feature_1 to develop as conflict
    # go on the feature branch
    git checkout feature_1
    
    # update graph history
    git fetch
    
    # start rebase
    git rebase -p develop
    
    # for each commit, fix conflict then
    git rebase --continue
    
    # push the new graph history of feature_1
    git push --force
```

If you made a mistake during rebase, you can abort it and start for scratch

```bash
    # for rebase not finish iet
    git rebase --abort
    
    # for rebase already finish
    git reset --hard origin/feature_1
```

## Revert

The `git revert` command can be considered an 'undo' type command, however, 
it is not a traditional undo operation. Instead of removing the commit from the 
project history, it figures out how to invert the changes introduced by the 
commit and appends a new commit with the resulting inverse content. 

This prevents Git from losing history, 
which is important for the integrity of your revision history and for reliable collaboration.

This as some side effect, which need to be kept in mind:
- reverting create a **new commit**: graph integrity is kept unlike `git reset` or `git rebase -i`
- In case of **reverting a merge**, and want to merge it back with 
modification: you first need to **revert** the **revert commit** and then merge back the feature.

## Cherry-pick

The `git cherry-pick` command can be considered an 'clone' type command, 
it is not a traditional 'copy' operation. Instead of adding the commit same 
from to the project history, it make a new commit with the same changes. 

Given one or more existing commits, it apply the change each one introduces, 
recording a new commit for each.

## Git ignore

### .gitignore

This file can be found anywhere in the project. it ignore or force files to be follow by git.

```git exclude
    # ignore all files with path containing .phpunit
    .phpunit
    
    # ignore only the .phpunit file next to .gitignore
    /.phpunit
    
    # force all files with path containing .phpunit to be follow
    !.phpunit
    
    # force only the .phpunit file next to .gitignore to be follow
    !/.phpunit
```

A common tips, to ignore a folder but force git to create it

```git exclude
    /path/to/ignore
    !/path/to/ignore/.gitkeep
```

### .gitignore_global

You can add a .gitignore_global on the ~/.gitconfig

    [core]
        excludesfile = ~/.gitignore_global
        
By convention, all IDE related file MUST be ignore here, never withing the .gitignore for the projet

## make a pull request

- Create a new branch to work on.
- Implement/fix your feature, comment your code.
- Follow the code style of the project, including indentation.
- Always commit check code (PSR-2, PHPmd), or your pull request may be decline.
- Commit every hours (or more), with correct and understandable messages.
- Push your branch after each commit.
- Once finish, go on bitbucket and make your pull request to `develop`
- (optional) Correct your pull request according to the review
- Once the pull request is approved and merged you can pull `develop`.

And last but not least: Your commit message should describe what the commit, 
when applied, does to the code – not what you did to the code.

## Go deeper

If all this powers are not enough for you, you may have a look to 
some [git voodoo magic](https://github.com/git-tips/tips)
