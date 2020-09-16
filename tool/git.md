# Git

Git is the most commonly used version control system out there. In short its
principle is that each commit represents the state of the codebase at that point
in time. Thus for work to be done commits must be made and well... comitted.

Make sure you are familiar with the basic commands, [complete documentation can
be found here](https://git-scm.com/docs).

TODO example commands

## Configuration

TODO

## Branches

## Commits

Commits should be atomic; a quality we canonically describe as "indiviudally
useful and complete". For example, if you are fixing a bug the fix for that bug
should take place on a single commit only. [See squashing for a more complete
example scenario](#squashing).

### Squashing

!> Never squash a public branch unless there is agreed upon very good reasons to
do so. Squashing rewrites history.

Squashing should only be used to reduce non-atomic commits into atomic ones. All
other uses of squashing introduce side effects which can potentially cause
problems with team workflow unless all participants are hyper Git experts.

Non-atomic commits are usually (intentionally) the consequence of preferring to
commit intermediate work as opossed to stashing, there is no problem with this
as long as these intermediate non-atomic commits are squashed.

?> **Example** You are fixing a bug. You are trying various solutions out. You
want to save each version of your attempt for potentially useful code to
copy later on. 7 commits down the line you solve the bug and condense all your
code and tidy it up. Squash those 7 commits into the single one that fixes the
bug.

### Rebasing

!> Never squash a public branch unless there is agreed upon very good reasons to
do so. Rebasing rewrites history.

## FAQ

### Rewriting history

Rewriting history (on public branches) is bad. The power of git is its
preservation of history
which can be exploited through commands such as `git blame` and `git log -p`,
i.e. "who did this change so I can either nag, get insight from, or ruthlessly
crush them for being so silly" (do not do the last one) and "how has this file
changed over time?". This is useful for a myriad of reasons. Things like
squashes or rebasing rewrite history and alter these logs which, usually,
diminishes the value provided by git.

There are limited goods reasons to rewrite history and use should be sparse.
Perhaps you need to squash commits you forgot to for instance. Needing to
frequently do so may be symptomatic of other problems.

### What is a "public branch"?

A public branch is one that is _intended_ to be shared between people. This
is a semantic difference. Yes you can push your local development branches to
the remote so you can fetch them between machines (remember to close them once
done to prevent branch pollution). These branches would functionally be private.
Example public branches would be the `master` branch, the `develop` branch or
any branch related to a >1 person effort.
