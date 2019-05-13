---
layout: posts
title:  "Why? What? How?"
categories: [git]
tags: [git]
---

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
    requests without confusion. You can iterate without polluting the master
    branch with potentially unstable, unfinished code. 
    [read more...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
    
* Branch out from `develop`
    
    >This way, you can make sure that code in master will almost always
    build without problems, and can be mostly used directly for releases 
    (this might be overkill for some projects).

* Never push into `develop` or `master` branch. Make a Pull Request.
    
    > It notifies team members that they have completed a feature. It also
    enables easy peer-review of the code and dedicates forum for discussing
    the proposed feature.

* Update your local `develop` branch and do an interactive rebase before
pushing your feature and making a Pull Request.

    > Rebasing will merge in the requested branch (`master` or `develop`) 
    and apply the commits that you have made locally to the top of the history
    without creating a merge commit (assuming there were no conflicts). 
    Resulting in a nice and clean history.
    [read more ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Resolve potential conflicts while rebasing and before making a Pull Request.
* Delete local and remote feature branches after merging.
    
    > It will clutter up your list of branches with dead branches. It ensures
    you only ever merge the branch back into (`master` or `develop`) once. 
    Feature branches should only exist while the work is still in progress.

* Before making a Pull Request, make sure your feature branch builds successfully
and passes all tests (including code style checks).
    
    > You are about to add your code to a stable branch. If your feature-branch
    tests fail, there is a high chance that your destination branch build will 
    fail too. Additionally, you need to apply code style check before making a 
    Pull Request. It aids readability and reduces the chance of formatting fixes 
    being mingled in with actual changes.

* Protect your `develop` and `master` branch.
  
    > It protects your production-ready branches from receiving unexpected and 
    irreversible changes. read more... 
    [Github](https://help.github.com/articles/about-protected-branches/), 
    [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html)
    and [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)
