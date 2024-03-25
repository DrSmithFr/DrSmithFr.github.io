---
layout: posts
title:  "Basic commands"
categories: [git]
tags: [git]
---

Keep in mind this just an overview of how each command interact with git.

![git resume](https://i.stack.imgur.com/MgaV9.png)

## git init
 
To set up a new repository, we need to open a terminal, navigate to our project
directory and run `git init`. This will enable Git for this particular folder 
and create a hidden `.git` directory where the repository history and 
configuration will be stored.

## git clone
```bash
    git clone https://github.com/emas/example-project.git local_project_dir_name
```

A new local repository is automatically created, with the github version configured as a remote.

## git add

Add files to the current index.

```bash
    git add path/to/file.ext
```

You can use the famous shortcut to add everything to the index

```bash
    git add .
```

## git reset

Remove files form the current index, but keep modification.

```bash
    git reset path/to/file.ext
```

Remove files from the current index and erase modification

```bash
    git reset --hard path/to/file.ext
```

Using the `--hard` flag is equivalent to use

```bash
    git reset path/to/file.ext && git checkout path/to/file.ext
```

You can use the famous shortcut to reset everything from the index

```bash
     git reset (--hard)
```

## git commit

Make a commit from the current index. 
You can directly add the message using `-m` option

You can directly add all untracked files using `-a` option 
(only already managed files will be added, new files will be ignore: use `git add .` instead)

Alias: `git ci`

## git fetch

this command will download the updated graph of commit.
Used with `-p` option, it will also delete local branch that have been destroy on server. 

## git pull

This command will perform as background a `git fetch` before "pulling" anything.
According to your current git configuration, it may perform a `merge` or a `rebase`,
to retrieve updated code.

This command may fail if your repository is not clean.
you can cleanup your workspace using `git reset --hard`, or keep it for later using `git stash save`

## git push

This command update the remote repository commit tree with the one from your local repository.

This command may fail if your not up to date on your branch. Use `git pull` and fix 
conflict to solve this. This command may also fail if you change the graph history. 
Use `git pull -f` to rewrite it on server. 

## git merge

Merging a branch involves pulling one branch into another branch while keeping the
original branching structure intact. Commits are thereby grouped as part of an 
encompassing feature or fix branch, and branches remain visible long after a branch
has been deleted.

This is especially useful when visibility and traceability is valued by your team in the long run.

![merge](https://wac-cdn.atlassian.com/dam/jcr:83323200-3c57-4c29-9b7e-e67e98745427/Branch-1.png?cdnVersion=jt)

This an example of Three-Way merge. 
Fast-forward merge have pretty much the same comportment as rebase did, but adding a merge commit.

To avoid Fast-forward merges, please always use the `--no-ff` option.

Example merging branch **feature_1** in **master**:

```bash
    git checkout masters
    git pull
    git merge --no-ff feature_1
    git push
```

## git rebase

A rebase changes the original base commit of a branch. 
This usually happens when a tracking branch points to an obsolete base as a result of 
other team members’ active contributions to the main branch of development.

In the process of a rebase, your commits are temporarily removed so that the base 
commit can be updated with upstream changes and your commits are then applied on 
top of the updated base commit. The sole purpose of a rebase is to maintain a 
linear history while integrating upstream changes into your local branch.

The diagram below visualizes this process.

![rebase](https://wac-cdn.atlassian.com/dam/jcr:e4a40899-636b-4988-9774-eaa8a440575b/02.svg?cdnVersion=kx)

In theory, if done correctly, rebase works well when working alone. When working
on a team with shared branches, however, it becomes much more complicated. One 
must be extremely careful when choosing to rebase. Rebase needed to be used only
to fix conflict on merge request. It allow you to solve the conflict for each commit 
(opposite to merge, which allow you to fixing conflict only with the merge commit).

Example rebasing branch **feature_1** from **master**

```bash
    git checkout feature_1
    git pull
    git rebase -p master
    
    # fix conflict
    git rebase --continue
    
    # As we rewrite graph history, we need to force push it
    git push -f
```

Now **feature_1** is up to date with **master** and they can be merge without conflicts!

## git branch

When developing a new feature, it is considered a good practice to work on a copy 
of the original project, called a branch. Branches have their own history and 
isolate their changes from one another, until you decide to merge them back together.

This is done for a couple of reasons:

- An already working, stable version of the code won't be broken.
- Many features can be safely developed at once by different people.
- Developers can work on their own branch, without the risk of their codebase
changing due to someone else's work.
- When unsure what's best, multiple versions of the same feature can be 
developed on separate branches and then compared.

You can create a new branch from the current one using:

```bash
    git branch branch_name
    # or
    git checkout -b branch_name
    # or with gitconfig alias
    git co -b branch_name
```

You can delete a local branch using:

```bash
    git branch -D branch_name
```

To delete remote branch, you need to use push

```bash
    git push origin :branch_name
```

## git checkout

This command allow a bunch of different thing
- switch branch
- download the current remote version of a file

For branch switching use:

```bash
    git checkout branch_name
    # or with gitconfig alias
    git co branch_name
```

Return to the last branch with
    
```bash
    git checkout -
    # or with gitconfig alias
    git co -
```

To reset a modified and untracked file

```bash
    git checkout path/to/file.ext
    # or with gitconfig alias
    git co path/to/file.ext
```

## Git hooks

Git hooks are shell scripts that trigger when you perform a specific action in Git.
They are useful tools for automated checks.
