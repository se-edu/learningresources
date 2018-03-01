# Best Practices with `git`

Author(s): [Darren Wee](https://github.com/darrenwee)

- [Introduction](#introduction)
- [Best Practices](#best-practices)
    - [Write Good Commit Messages](#write-good-commit-messages)
        - [What Constitutes a Good Commit Message](#what-constitutes-a-good-commit-message)
        - [Set Up Your Editor for Commit Messages](#set-up-your-editor-for-commit-messages)
    - [Always Commit Functional Code](#always-commit-functional-code)
        - [Stashing](#stashing)
    - [One Logical Change to One Commit](#one-logical-change-to-one-commit)
    - [Hide the Sausage Making](#hide-the-sausage-making)
        - [How to Hide the Sausage](#how-to-hide-the-sausage)
    - [Respect Published History](#respect-published-history)
        - [Exceptions](#exceptions)
    - [Keep Up To Date](#keep-up-to-date)
        - [Use Remotes Effectively](#use-remotes-effectively)
        - [Rebase versus Merging](#rebase-versus-merging)
- [Resources](#resources)
    - [Further Reading](#further-reading)


# Introduction
[`git`](https://git-scm.com/) is a popular source code management tool and commonly used in many open-source projects, especially those on [GitHub](https://github.com).

# Best Practices
`git` can be an incredibly useful tool for collaboration or it can be a terrible headache. Best practices exist in order to create a common understanding between users so that the latter does not happen.

Best practices are guidelines that are mostly sensible, but are still guidelines. You can always choose to ignore them although best if you have a compelling reason to do so.

---

## Write Good Commit Messages
Good commit messages can help:
- reviewers to understand the high-level changes in your pull request/patch
- others to understand your reasoning behind changes made by that commit,
    - while reviewing your code,
    - while figuring out why a piece of code that is five years old is that way,

### What Constitutes a Good Commit Message
The easiest way to attain commit message discipline is to stop putting in one-liner descriptions using `git commit -m "Add some things to that."`. Instead, use `git commit` and write a proper message in an editor.

A good commit message can be formatted the following way:
```
Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase can get confused if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent
```

Source: [A Note About Git Commit Messages by Tim Pope.](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

**Some Examples**
- [torvalds/subsurfance-for-dirk](https://github.com/torvalds/subsurface-for-dirk/commits/master)
- [torvalds/linux](https://github.com/torvalds/linux/commits/master)

### Set Up Your Editor for Commit Messages
1. To use your editor of choice for `git`-related functionality, e.g. `vim`, do one of either
```
git config --global core.editor "vim" # or you can do the following
export GIT_EDITOR=vim # add to your .bashrc
```
2. Set your editor to wrap after 72 characters. In `vim`, you can do this by adding this to your `.vimrc`:
```
syntax on
autocmd Filetype gitcommit spell textwidth=72
```
---
## Always Commit Functional Code
Merges to the following must always leave the project in a working state, i.e. it can be built and run on:
- `master` branch, or equivalent,
- `staging` branch, `development` branch or equivalent, if any.

Changes to your own branches _that no one else is using_ can have non-functioning commits. However, you may wish to [hide the sausage making](#hide-the-sausage-making) to squash non-functioning commits into a single, functioning commit before you make a pull request.

Changes to your own branches that is used by others should obey always-functioning-commits rule to minimize surprise. This is especially important if you expect your branch to be cherry-picked by another collaborator because they require a specific bit of code that you wrote.

### Stashing
If you need to switch between branches while in the middle of developing a commit, you can use the `git stash` command instead of committing your half-done code.

```bash
# stash your work not committed to HEAD yet by pushing it onto the stash stack
git stash
git stash push # equivalent to git stash

# restore your most recently stashed work to your current working copy
git stash pop

# acts like git stash pop, but keeps a copy of the stash in the current stash stack
git stash apply

# list all stashes in the stack
git stash list
```

Stashes are a purely local construct and cannot be pushed to a remote repository.

Read more: [git stash - Saving Changes | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/git-stash)

---

## One Logical Change to One Commit
Commits are the building blocks of a codebase; each building block should contribute exactly one useful thing, like:

- adding a new function or piece of data
- fixing one or more bugs
    - correcting incorrect behavior
- refactoring code or data
    - reorganizing code
    - removing typos
    - formatting code
    - changing representation of data to a different format

Each _logical change in code_ should translate to exactly one commit, nothing more or less. Doing so allows you to:
- revert a particular logical change with no/less side effects
- easily identify bad commits that caused change in behavior
- collaborate with others, e.g. by cherry-picking a single logical change instead of finding a bit of this commit and a bit of that commit
- package that logical change [with a useful commit message to explain why/how you did something](#write-good-commit-messages)

It may also become necessary to scope down what you may deem as a single logical change if it results in a very large commit, as that can also [introduce other problems](https://softwareengineering.stackexchange.com/a/10796). For example, implementing a single, new feature can be thought of as one logical change to the codebase, but making a pull request for a single, large commit also makes the above benefits disappear.

If you are concerned about appearances, you can always opt to [hide the sausage making](#hide-the-sausage-making) to clean up your commit history.

If you have made several overlapping changes on your working directory (e.g. forgot to commit, etc), you can always [perform a patch-wise stage using `git add -p`](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git).

Read more:
- [programming practices - When to commit code?](https://softwareengineering.stackexchange.com/questions/83837/when-to-commit-code)
- [Do commit early and often](https://sethrobertson.github.io/GitBestPractices/#commit)
- [When is a version control commit too large?](https://softwareengineering.stackexchange.com/questions/10793/when-is-a-version-control-commit-too-large)
- [Commit only part of a file in Git](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git) _this is useful to use as a cheatsheet during interactive staging_

---

## Hide the Sausage Making
_Sausage making_ refers to the process by which code is incrementally worked on, where a series of commits (like links in a sausage) make up a branch. It is often desirable to _hide the sausage making_ where the commit history is cleaned up so that it looks neater and is easier to follow.

When working on a `feature`/`fix` branch, you may
- [commit non-functioning code](#always-commit-functional-code), or
- may need [multiple commits to implement one logical change in code](#one-logical-change-to-one-commit), or
- cherry-pick a lot of commits from other branches

This may clutter your history with low-level details or make it convoluted to follow for a maintainer or reviewer. Like sausage, you may enjoy eating it but not the process of making it.

### How to Hide the Sausage
Hiding the sausage is typically achieved by either/both:
- performing an interactive rebase, i.e. `git rebase -i`
- patch-wise reset and stage, i.e. `git reset -p` and `git add -p`

Ensure that you do this _before_
- pushing to a remote repository [to respect the published history](#respect-published-history) and
- performing any merges from other branches.

Read more:
- [On Sausage Making](https://sethrobertson.github.io/GitBestPractices/#sausage)
- [Git Tools - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) (great reference for how to actually do this, under _Changing Multiple Commit Messages_)

---

## Respect Published History
Always avoid rewriting the public, published history unless you have a very good reason, you are very sure of what you are doing, or you will not affect anyone else.

### Exceptions
You are working on your own branch that _no one else is using_, and
- you want to revert a commit without introducing another commit
- you are rebasing the branch
- you are [cleaning up the history of your branch](#hide-the-sausage-making)

Read more:
- [Rewriting History - Atlassian](https://www.atlassian.com/git/tutorials/rewriting-history)
- [Don't change public history.](https://sethrobertson.github.io/GitBestPractices/#pubonce)

---

## Keep Up To Date
### Use Remotes Effectively
_Remotes_ refer to versions of the project you are working on that are hosted elsewhere, usually on the Internet. Remotes are very handy for managing collaboration, e.g. if you have to keep your code in sync with the `upstream` branch of the project, or if you need to pull code from someone else which may not be merged yet.

You can have as many remotes as you want, each possibly being read-only or with read/write privileges.

Remotes are managed using the `git remote` command.
```bash
# view all remotes
git remote -v

# add a remote called "upstream" that points to https://github.com/TEAMMATES/teammates
git remote add upstream https://github.com/TEAMMATES/teammates

# branch off from the master branch of the upstream repository
git fetch upstream # get data from upstream repo
git checkout -b your-fancy-branch upstream/master # makes a new branch off the head of upstream/master
```

Read more:
- [Git Basics - Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

### Rebase versus Merging
| You should... | when ... |
|-------------|-|
| merge | you created a branch to develop a feature, and now you want these changes to be inside `master` |
| rebase | you created a branch from `master` to develop a feature, and someone else pushed a change to `master` before you finished |

It is generally considered good practice to rebase your feature branch onto whatever branch you're trying to patch _before_ you make the pull request, resolving any conflicts that arise. This:

- [keeps the history clear and linear](http://www.bitsnbites.eu/a-tidy-linear-git-history/).
    - makes backtracking easier
    - easy to follow history
    - reverting/rolling back is much simpler
    - you can use `git bisect` to find regressions easily
- ensures your changes are compatible with the head of the branch you're patching
- makes reviewing/testing easier by [not including irrelevant code by merging](https://lwn.net/Articles/328436/)

Read more:
- [A Tidy, Linear Git History](http://www.bitsnbites.eu/a-tidy-linear-git-history/) _this is an excellent article which has formed my opinion on rebase vs. merge_
- [Rebasing and merging: some git best practices](https://lwn.net/Articles/328436/) _involves wisdom from Linus Torvalds_
- [Merging vs. Rebasing - Atlassian](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [When do you use git rebase instead of git merge - StackOverflow](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge/804178#804178)
---

# Resources
These are the resources used in the writing of this chapter, as well as any additional, interesting readings.

- [A Note About Git Commit Messages by Tim Pope](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [Git Best Practices by Seth Robertson](https://sethrobertson.github.io/GitBestPractices/)
- [A Tidy, Linear Git History](http://www.bitsnbites.eu/a-tidy-linear-git-history/)
- [Rebasing and merging: some git best practices](https://lwn.net/Articles/328436/) _involves wisdom from Linus Torvalds_

- [programming practices - When to commit code?](https://softwareengineering.stackexchange.com/questions/83837/when-to-commit-code)
- [When is a version control commit too large?](https://softwareengineering.stackexchange.com/questions/10793/when-is-a-version-control-commit-too-large)
- [Commit only part of a file in Git](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git) _this is useful to use as a cheatsheet during interactive staging_
- [When do you use git rebase instead of git merge - StackOverflow](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge/804178#804178)

- [Git Tools - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [Git Basics - Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

- [git stash - Saving Changes | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/git-stash)
- [Merging vs. Rebasing - Atlassian](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [Rewriting History - Atlassian](https://www.atlassian.com/git/tutorials/rewriting-history)

## Further Reading
- [Git for Computer Scientists](http://eagain.net/articles/git-for-computer-scientists/)
- [Pro Git](https://git-scm.com/book/en/v2)