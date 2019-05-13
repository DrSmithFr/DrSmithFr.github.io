---
layout: posts
title:  "Best practices"
categories: [git]
tags: [git]
---

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
