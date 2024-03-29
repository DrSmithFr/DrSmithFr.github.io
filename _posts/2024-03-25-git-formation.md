---
layout: posts
title:  "Git Basic Formation"
categories: [git]
tags: [git]
---

# Why? What? How?

Git is a collection of command line utilities that track and record
changes in files. 

With it you can restore old versions of your project, compare,
analyze, merge changes and more. 
This process is referred to as version control. 

Git is decentralized, which means that it doesn't depend on a central
 server to keep old versions
of your files. Instead it works fully locally by storing this data as
 a folder on your hard drive,
which we call a repository.

## Commit (aka Snapshot, aka Revision)

A commit is a textual representation of change append to files.

A commit is defined by few data
- hash uniq identifier (sha1)
- parent hash uniq identifier
- commit message
- commit date
- commit author
- textual representation of change

This textual representation is done by only few directives
- create file
- remove file
- move file
- line added at
- line remove at

This textual representation is more or less what you get using `git diff`.

## History graph

Git use an internal graph to store repository history.
Each commit is a node of this graph and MUST have only one parent.

## Index

The index can be simply explain as a "future commit". 
It contain the textual representation of change added to the index (using `git add path/to/file.ext`).

Using `git status`, show content of the index with all files flagged as tracked.
Alias: `git st`

## Repository

A repository is a folder containing a `.git` directory.
This directory contain all the data needed to manage the repository.

## Remote repository

A remote repository is a repository on a server.

## Installation

Installing Git on your machine is straightforward:

- **Linux** - Simply open up a new terminal and install git via your
distribution's package manager. 
For Ubuntu the command is: `sudo apt-get install git`
- **Windows** - we recommend [git for windows](https://gitforwindows.org/)
as it offers both a GUI client and 
a BASH comand line emulator.
- **OS X** - The easiest way is to install [homebrew](https://brew.sh/),
and then just run brew install 
git from your terminal.

If you are an absolute beginner, then a graphical git client is a must. 
Since you are an absolute killer at programming you need to use and 
understand every command of git.

## Configuration

There are a lot of options that can be fiddled with, but we are going to 
set up the most important ones: 
our username and email.
Open a terminal and run these commands:

```bash
    git config --global user.name "My Name"
    git config --global user.email myEmail@example.com
```

You can also custom git configuration editing `~/.gitconfig` file.

> [Here is an example](/git-config/) of advanced configuration.

## Rules

There are a set of rules to keep in mind:

* Perform work in a feature branch.
    
    >Because this way all work is done in isolation on a dedicated branch
    rather than the main branch. It allows you to submit multiple pull 
    requests without confusion. You can iterate without polluting the main
    branch with potentially unstable, unfinished code. 
    [read more...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
    
* Branch out from `dev`
    
    >This way, you can make sure that code in main will almost always
    build without problems, and can be mostly used directly for releases 
    (this might be overkill for some projects).

* Never push into `dev` or `main` branch. Make a Pull Request.
    
    > It notifies team members that they have completed a feature. It also
    enables easy peer-review of the code and dedicates forum for discussing
    the proposed feature.

* Update your local `dev` branch and do an interactive rebase before
pushing your feature and making a Pull Request.

    > Rebasing will merge in the requested branch (`main` or `dev`) 
    and apply the commits that you have made locally to the top of the history
    without creating a merge commit (assuming there were no conflicts). 
    Resulting in a nice and clean history.
    [read more ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Resolve potential conflicts while rebasing and before making a Pull Request.
* Delete local and remote feature branches after merging.
    
    > It will clutter up your list of branches with dead branches. It ensures
    you only ever merge the branch back into (`main` or `dev`) once. 
    Feature branches should only exist while the work is still in progress.

* Before making a Pull Request, make sure your feature branch builds successfully
and passes all tests (including code style checks).
    
    > You are about to add your code to a stable branch. If your feature-branch
    tests fail, there is a high chance that your destination branch build will 
    fail too. Additionally, you need to apply code style check before making a 
    Pull Request. It aids readability and reduces the chance of formatting fixes 
    being mingled in with actual changes.

* Protect your `dev` and `main` branch.
  
    > It protects your production-ready branches from receiving unexpected and 
    irreversible changes. read more... 
    [Github](https://help.github.com/articles/about-protected-branches/), 
    [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html)
    and [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)


# GitHub - Overview

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

Example merging branch **feature_1** in **main**:

```bash
    git checkout main
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

Example rebasing branch **feature_1** from **main**

```bash
    git checkout feature_1
    git pull
    git rebase -p main
    
    # fix conflict
    git rebase --continue
    
    # As we rewrite graph history, we need to force push it
    git push -f
```

Now **feature_1** is up to date with **main** and they can be merge without conflicts!

## Git hooks

Git hooks are shell scripts that trigger when you perform a specific action in Git.
They are useful tools for automated checks.

# Git Common Operation

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
So your branch may be created from `dev` or `main` on this project.

- Branch create from `dev` can be merge in `dev` only. 
- Branch create from `main` can be merge in `main` or `dev`.

So:

- **Hot fix branch** are always created from `main`.
- **bug fix branch** are always created from `dev`.
- **Feature branch** may be create from `dev` or `main`, according to which 
merging strategy need to be apply.

### Commands

```bash
    # go on main or dev
    git checkout main|dev
    
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
    git commit --amend
    # or with gitconfig alias
    git amend
```

Add current index to the last commit and keep old message

```bash
    git commit --amend --no-edit
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
    stash@{0}: On main: stash_message
    stash@{1}: On dev: stash_message
    
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
    # my merge request from feature_1 to dev as conflict
    # go on the feature branch
    git checkout feature_1
    
    # update graph history
    git fetch
    
    # start rebase
    git rebase -p dev
    
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

```bash
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


```bash
    /path/to/ignore
    !/path/to/ignore/.gitkeep
```

### .gitignore_global

You can add a .gitignore_global on the ~/.gitconfig

```bash
    [core]
        excludesfile = ~/.gitignore_global
```
        
By convention, all IDE related file MUST be ignore here, never withing the .gitignore for the projet

## make a pull request

- Create a new branch to work on.
- Implement/fix your feature, comment your code.
- Follow the code style of the project, including indentation.
- Always commit check code (PSR-2, PHPmd), or your pull request may be decline.
- Commit every hours (or more), with correct and understandable messages.
- Push your branch after each commit.
- Once finish, go on bitbucket and make your pull request to `dev`
- (optional) Correct your pull request according to the review
- Once the pull request is approved and merged you can pull `dev`.

And last but not least: Your commit message should describe what the commit, 
when applied, does to the code – not what you did to the code.

## Go deeper

If all this powers are not enough for you, you may have a look to 
some [git voodoo magic](https://github.com/git-tips/tips)


# Git Best Practices

Be a good developer, follow the guideline.

## Commit early and often

Git only takes full responsibility for your data when you commit.  If
you fail to commit and then do something poorly thought out, you can
run into trouble.  Additionally, having periodic checkpoints means
that you can understand how you broke something.

People resist this out of some sense that this is ugly, limits
`git-bisect`ion functionality, is confusing to observers, and might
lead to accusations of stupidity.  Well, I'm here to tell you that
resisting this is ignorant.  *Commit Early And Often*.  If, after you
are done, you want to pretend to the outside world that your work
sprung complete from your mind into the repository in utter perfection
with each concept fully thought out and divided into individual
concept-commits, well git supports that: see Sausage Making below.
However, don't let tomorrow's beauty stop you from performing
continuous commits today.

Personally, I commit early and often and then let the sausage making
be seen by all except in the most formal of circumstances.  Just look
at the history of this gist!

## Don't panic

As long as you have committed your work (or in many cases even added
it with `git add`) your work will not be lost for at least two weeks
unless you really work at it (run commands which manually purge it).

When attempting to find your lost commits, first make *sure* you will
not lose any current work.  You should commit or stash your current
work before performing any recovery efforts which might destroy your
current work.  After finding the commits you can reset, rebase,
cherry-pick, merge, or otherwise do what is necessary to get the
commit history and work tree you desire.

There are three places where "lost" changes can be hiding. They might be
in the reflog (`git log -g`), they might be in lost&found (`git fsck
--unreachable`), or they might have been stashed (`git stash list`).

* reflog

    The reflog is where you should look first and by default.  It
    shows you each commit which modified the git repository.  You can
    use it to find the commit name (SHA-1) of the state of the
    repository before (and after) you typed that command.  While you
    are free to go through the reflog manually, you can also visualize
    the repository using the following command (Look for dots without
    children and without green labels):

```bash
    gitk --all --date-order $(git log -g --pretty=%H)
```

* Lost and found

    Commits or other git data which are no longer reachable though any
    reference name (branch, tag, etc) are called "dangling" and may be
    found using fsck.  There are legitimate reasons why objects may be
    dangling through standard actions and normally over 99% of them are
    entirely uninteresting for this reason.

    * Dangling Commit

    These are the most likely candidates for finding lost data. A
    dangling commit is a commit no longer reachable by any branch or
    tag. This can happen due to resets and rebases and are
    normal. `git show SHA` will let you inspect them.

    The following command helps you visualize these dangling
    commits. Look for dots without children and without green labels.

```bash
    gitk --all --date-order $(git fsck --no-reflog | grep "dangling commit" | awk '{print $3;}')
```

* Dangling Blob

A dangling blob is a file which was not attached to a commit. This
is often caused by `git add`s which were superceded before commit
or merge conflicts. Inspect these files with

```bash
    git show SHA
```

* Dangling Tree

A dangling tree is a directory tree of files that was not attached
to a commit. These are rarely interesting, and often caused by
merge conflicts. Inspect these files with `git ls-tree -r SHA`

* Stashes

    Finally, you may have stashed the data instead of committing it and
    then forgotten about it.  You can use the `git stash list` command
    or inspect them visually using:

```bash
    gitk --all --date-order $(git stash list | awk -F: '{print $1};')
```

* Misplaced

    Another option is that your commit is not lost.  Perhaps the
    commit was just made on a different branch from what you remember.
    Using `git log -Sfoo --all` and `gitk --all --date-order` to try
    and hunt for your commits on known branches.

* Look elsewhere

    Finally, you should check your backups, testing copies, ask the other
    people who have a copy of the repo, and look in other repos.
    
## Useful commit messages

Creating insightful and descriptive commit messages is one of the best
things you can do for others who use the repository.  It lets people
quickly understand changes without having to read code.  When doing
history archeology to answer some question, good commit messages
likewise become very important.

The normal git rule of using the first line to provide a short (72
character) summary of the change is also very good.  Looking at the
output of `gitk` or `git log --oneline` might help you understand why.

Here are 
some rules of thumb ([source](https://chris.beams.io/posts/git-commit/#seven-rules)):

* Separate the subject from the body with a newline between the two.

> Git is smart enough to distinguish the first line of your commit
  message as your summary. In fact, if you try git shortlog, instead
  of git log, you will see a long list of commit messages, consisting
  of the id of the commit, and the summary only.

* Limit the subject line to 50 characters and Wrap the body at 72 characters.

> Commits should be as fine-grained and focused as possible, it is 
  not the place to be verbose. [read more...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

* Capitalize the subject line.
* Do not end the subject line with a period.
* Use [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) in the subject line.

> Rather than writing messages that say what a committer has done.
  It's better to consider these messages as the instructions for what
  is going to be done after the commit is applied on the repository. 
  [read more...](https://news.ycombinator.com/item?id=2079612)

* Use the body to explain **what** and **why** as opposed to **how**.

## Keeping up to date

This section has some overlap with workflow.  Exactly how and when you
update your branches and repositories is very much associated with the
desired workflow.  Also I will note that not everyone agrees with
these ideas (but they should!)

* Pulling with --rebase

Whenever I pull, under most circumstances I `git pull --rebase`. This
is because I like to see a linear history (my commit came after all
commits that were pushed before it, instead of being developed in
parallel).  It makes history visualization much simpler and `git
bisect` easier to see and understand.

Some people argue against this because the non-final commits may lose
whatever testing those non-final commits might have had since the
deltas would be applied to a new base.  This in turn might make
git-bisect's job harder since some commits might refer to broken
trees, but really this is only relevant to people who want to hide the
sausage making.  Of course to really hide the sausage making you
should still rebase (and test the intermediate commits, if any).

* Rebasing (when possible)

Whenever I have a private branch which I want to update, I use rebase
(for the same reasons as above).  History is clean and simple.
However, if you share this branch with other people, rebasing is
rewriting public history and should/must be avoided.  You may only
rebase commits that no-one else has seen (which is why `git pull
--rebase` is safe).

* Merging without speeding

`git merge` has the concept of fast-forwarding, or realizing that the
code you are trying to merge in is identical to the result of the code
after the merge.  Thus instead of doing work, creating new commits,
etc, git simply changes the branch pointers (fast forwards them) and
calls it good.

This is good when doing `git pull` but not so good when doing `git
merge` with a non-@{u} (upstream) branch.  The reason this is not good
is because it loses information.  Specifically it loses track of which
branch is the first parent and which is not.  If you don't ever want
to look back into history, then it does not matter.  However, if you
might want to say "which branch was this commit originally committed
onto," if you use fast-forwarding that question is impossible to
answer since git will pick one branch or the other (the first parent
or second parent) as the one which both branches activities were
performed on and the other (original) parent's branch will be
anonymous.  There are typically worse things in the world, but you
lose information that is not recoverable in any other way by a
repository observer and in my book that is bad.  Use `git merge
--no-ff`
