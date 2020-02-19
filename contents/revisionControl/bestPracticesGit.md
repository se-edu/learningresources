<frontmatter>
  title: Best Practices with `git`
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Best Practices with `git`

Author(s): [Darren Wee](https://github.com/darrenwee)

<box id="article-toc">

* [Best Practices with git‎](#best-practices-with-git)
* [Introduction‎](#introduction)
* [Best Practices‎](#best-practices)
    * [Write Good Commit Messages‎](#write-good-commit-messages)
        * [What Constitutes a Good Commit Message‎](#what-constitutes-a-good-commit-message)
        * [Set Up Your Editor for Commit Messages‎](#set-up-your-editor-for-commit-messages)
    * [Always Commit Functional Code‎](#always-commit-functional-code)
        * [Stashing‎](#stashing)
    * [One Logical Change per Commit‎](#one-logical-change-per-commit)
    * [Hide the Sausage Making‎](#hide-the-sausage-making)
        * [How to Hide the Sausage Making‎](#how-to-hide-the-sausage-making)
    * [Respect Published History‎](#respect-published-history)
    * [Keep Up To Date‎](#keep-up-to-date)
        * [Working with Remotes‎](#working-with-remotes)
        * [Rebase versus Merging‎](#rebase-versus-merging)
* [Resources‎](#resources)
    * [Further Reading‎](#further-reading)
</box>

# Introduction
[`git`](https://git-scm.com/) is a popular source code management tool and commonly used in many open-source projects, especially those on [GitHub](https://github.com).

# Best Practices
`git` can be an incredibly useful tool for collaboration or it can be a terrible headache. Best practices exist in order to create a common understanding between users so that the latter does not happen.

Best practices are guidelines that are mostly sensible, but are still guidelines. You can always choose to ignore them although best if you have a compelling reason to do so.

---

## Write Good Commit Messages
Good commit messages can help reviewers or other contributors to understand:
- the high-level changes made by your pull request/patch
- the reasoning behind the changes made by that commit while
    - reviewing your code
    - figuring out why a piece of code that is five years old is that way

They also assist you in the development process if you forget what has been done, or if you need to cherry-pick commits for elsewhere.

### What Constitutes a Good Commit Message
The easiest way to attain commit message discipline is to stop putting in one-liner descriptions using `git commit -m "Add some things to that."`. Instead, write a proper commit message in an editor:

```bash
# opens your editor to write a commit message properly
git add files-to-stage
git commit

# like above, but shows the diff of the currently staged files
git add files-to-stage
git commit --verbose

# amend the most recent commit message
git commit --amend HEAD^
```

Every commit must have a well written commit message _subject line_.

1. Try to limit the subject line to 50 characters (hard limit: 72 chars)
    - Usually, only the subject line is shown in the log, conflict resolution, interactive rebase, etc.

2. Capitalize the subject line e.g. `Move index.html file to root`
    - Do not end the subject line with a period.

3. Use the imperative mood in the subject line
    - e.g. `Add README.md` rather than `Added README.md` or `Adding README.md` or `Adds README.md`.

4. Use `{scope}: {change}` format when applicable
    - e.g. `Person class: remove static imports`, or `Unit tests: remove blank lines`

5. Commit messages for non-trivial commits should have a *body* giving details of the commit.
    1. Separate subject from body with a blank line
    2. Wrap the body at 72 characters
    3. Use the body to explain:
        - _what_ the commit does, and
        - _why_ it was done that way, such that
        - the reader can refer to the diff to understand _how_ the change was done.
    4. Avoid including information that can be included in the code as comments.

Give an explanation for the change(s) that is detailed enough so that the reader can judge if it is a good thing to do, without reading the actual diff to determine how well the code does what the explanation promises to do. If your description starts to get too long, that’s a sign that you probably need to split up your commit to finer grained pieces.

Commit messages need to be wrapped to 72 characters or less so that the entire message can be shown without overflow on a standard, 80-column terminal while leaving room for indents/nested reply indicators if you pass `.patch` or `.diff` files via traditional mailing list ([source](https://stackoverflow.com/a/2120040/5399892)).

Read more: [Formats and Conventions: Commit Messages](https://oss-generic.github.io/process/docs/FormatsAndConventions.html#commit-message)

As a litmus test, you can try to read your commit message summary in the following manner:
> If applied, this commit will `your commit message summary here`

For example:
> If applied, this commit will `implement getHash() functionality in HashHelper`.

#### Examples of Good Commit Messages
Adapted from [se-edu/addressbook-level4](https://github.com/se-edu/addressbook-level4/commits/master) ([patch](https://github.com/se-edu/addressbook-level4/commit/2f4405c75cd21111952565a9706a9793b475c41e.patch)).
This commit message follows the guidelines above and also includes the context of the change (how it worked before this patch) as it is necessary to understand _why_ it needed to change.
```
UniquePersonList#remove(Person): update return type

UniquePersonList#remove(Person) returns true if the person passed into
this method can be found in the internal list, and false otherwise. It
also throws PersonNotFoundException if a person is not found.

Returning a boolean is not required as the exception is thrown before
the value is returned.

Let's update the return type for UniquePersonList#remove(Person) to
void.
```

Adapted from [torvalds/linux](https://github.com/torvalds/linux/commits/master) ([patch](https://github.com/torvalds/linux/commit/9fe8f03bc0227fb573cc3e5b99eb34e19e405ab6.patch)).
```
drm/amd/display: Fix memleaks when atomic check fails

While checking plane states for updates during atomic check, we create
dc_plane_states in preparation. These dc states should be freed if
something errors.

Although the input transfer function is also freed by
dc_plane_state_release(), we should free it (on error) under the same
scope as where it is created.
```

More examples can be found here: [Formats and Conventions: Commit Messages](https://oss-generic.github.io/process/docs/FormatsAndConventions.html#commit-message)

### Set Up Your Editor for Commit Messages
1. To use your editor of choice for `git`-related functionality, e.g. `vim`, do one of either in your terminal:
```bash
git config --global core.editor "vim" # or you can do the following
export GIT_EDITOR=vim # add to your .bashrc or equivalent
```

2. Set your editor to wrap after 72 characters. In `vim`, you can do this by adding this to your `.vimrc`:
```bash
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
If you need to switch between branches while in the middle of developing a commit, you can use the `git stash` command. Stashing saves the uncommitted changes made in your current working directly. This allows you to save your progress without having to commit non-functioning code.

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

Read more:
- [git stash - Saving Changes | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/git-stash)

---

## One Logical Change per Commit
Commits are the building blocks of a codebase; each building block should contribute exactly one useful thing, like:
- adding a new function or piece of data
- fixing a bug
- refactoring code or data
    - reorganizing code
    - removing typos
    - formatting code
    - changing representation of data to a different format

Each _logical change in code_ should translate to exactly one commit, nothing more or less. Doing so allows you to:
- revert a particular logical change with little to no side effects
- easily identify bad commits that caused change in behavior
- collaborate with others easily, e.g. by cherry-picking a single logical change instead of finding a bit of this commit and a bit of that commit
- package that logical change [with a useful commit message to explain why/how you did something](#write-good-commit-messages)

It may also become necessary to scope down what you may deem as a single logical change if it results in a very large commit, as that can also [introduce other problems](https://softwareengineering.stackexchange.com/a/10796). For example, implementing a single, new feature can be thought of as one logical change to the codebase, but making a pull request for a single, large commit also makes the above benefits disappear.

If you are concerned about appearances, you can always opt to [hide the sausage making](#hide-the-sausage-making) to clean up your commit history.

If you have made several overlapping changes on your working directory (e.g. forgot to commit, etc), you can always [perform a patch-wise stage using `git add -p`](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git).

Read more:
- [Do commit early and often](https://sethrobertson.github.io/GitBestPractices/#commit) - an auxiliary best practice of frequency of making commits
- [programming practices - When to commit code?](https://softwareengineering.stackexchange.com/questions/83837/when-to-commit-code)
- [When is a version control commit too large?](https://softwareengineering.stackexchange.com/questions/10793/when-is-a-version-control-commit-too-large) - a discussion on judging the suitable size of a commit in different settings
- [Commit only part of a file in Git](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git) - this is useful to use as a cheatsheet during interactive staging

---

## Hide the Sausage Making
_Sausage making_ refers to the process by which code is incrementally worked on, where a series of commits (like links in a sausage) make up a branch. It is often desirable to _hide the sausage making_ where the commit history is cleaned up so that it looks neater and is easier to follow.

When working on a `feature`/`fix` branch, you may:
- [commit non-functioning code](#always-commit-functional-code), or
- may need [multiple commits to implement one logical change in code](#one-logical-change-to-one-commit), or
- cherry-pick a lot of commits from other branches

This may clutter your history with low-level details or make it convoluted to follow for a maintainer or reviewer. Like sausage, you may enjoy eating it but not the process of making it.

### How to Hide the Sausage Making
Hiding the sausage is typically achieved by either/both:
- performing an interactive rebase, i.e. `git rebase -i`
- patch-wise reset and stage, i.e. `git reset -p` and `git add -p`

Ensure that you do this _before_:
- pushing to a remote repository [to respect the published history](#respect-published-history) and
- performing any merges from other branches.

Read more:
- [On Sausage Making](https://sethrobertson.github.io/GitBestPractices/#sausage)
- [Git Tools - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) (great reference for how to actually do this, under _Changing Multiple Commit Messages_)

---

## Respect Published History
Always avoid rewriting the published history unless you are very sure of what you are doing, like:
- You are working on your own branch that _no one else is using_, and
    - you want to revert a commit without introducing another commit
    - you are rebasing the branch
    - you are [cleaning up the history of your branch](#hide-the-sausage-making)

A failed `git push` usually means that your local branch is behind its remote counterpart, indicating that the local and remote branches have diverged.

```bash
$ git push origin my-branch
To git@github.com:foo/foo.git
 ! [rejected]        my-branch -> my-branch (non-fast-forward)
error: failed to push some refs to 'git@github.com:foo/foo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Alternatively, you may also see this when a branch diversion has occurred when you run `git status`.

```bash
$ git status
Your branch and 'origin/my-branch' have diverged,
and have 3 and 5 different commit(s) each, respectively.
```

You can override this by [making a force push](https://stackoverflow.com/questions/10510462/force-git-push-to-overwrite-remote-files), i.e. `git push --force` but that would result in rewriting the published history or overwrite changes in the divergent remote commits. Observe the guidelines and ensure that the force push can be made in good faith with respect to your collaborators.

Read more:
- [Rewriting History - Atlassian](https://www.atlassian.com/git/tutorials/rewriting-history)
- [Don't change public history.](https://sethrobertson.github.io/GitBestPractices/#pubonce)
- [Force "git push" to overwrite remote files - StackOverflow](https://stackoverflow.com/questions/10510462/force-git-push-to-overwrite-remote-files)

---

## Keep Up To Date
### Working with Remotes
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

# change the URL for the upstream remote from HTTPS to SSH
git remote set-url upstream git@github.com:TEAMMATES/teammates.git

# remove a remote named "upstream"
git remote remove upstream
```

Read more:
- [Git Basics - Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

### Rebase versus Merging
| You should... | When ... |
|---------------|----------|
| merge | you created a branch to develop a feature, and now you want these changes to be inside `master` |
| rebase | you created a branch from `master` to develop a feature, and someone else pushed a change to `master` before you finished |

It is generally considered good practice to rebase your feature branch onto whatever branch you're trying to patch _before_ you make the pull request, resolving any conflicts that arise. This:
- [keeps the history clear and linear](http://www.bitsnbites.eu/a-tidy-linear-git-history/)
    - makes backtracking easier
    - easy to follow history
    - reverting/rolling back is much simpler
    - you can use `git bisect` to find regressions on your branch easily without involving unrelated changes from `master`
- ensures your changes are compatible with the head of the branch you're patching
- makes reviewing/testing easier by [not including irrelevant code by merging](https://lwn.net/Articles/328436/)

Read more:
- [A Tidy, Linear Git History](http://www.bitsnbites.eu/a-tidy-linear-git-history/) - this is an excellent article which has formed my opinion on rebase vs. merge
- [Rebasing and merging: some git best practices](https://lwn.net/Articles/328436/) - the merging/rebase issue involving wisdom from the creator of `git`, Linus Torvalds
- [Merging vs. Rebasing - Atlassian](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [When do you use git rebase instead of git merge - StackOverflow](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge/804178#804178) - a discussion on when to rebase and when to merge
---

# Resources
These are the resources used in the writing of this chapter, as well as any additional, interesting readings.

- [A Note About Git Commit Messages by Tim Pope](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [Git Best Practices by Seth Robertson](https://sethrobertson.github.io/GitBestPractices/)
- [A Tidy, Linear Git History](http://www.bitsnbites.eu/a-tidy-linear-git-history/)
- [Rebasing and merging: some git best practices](https://lwn.net/Articles/328436/) involves wisdom from Linus Torvalds
- [Formats and Conventions: Commit Messages](https://oss-generic.github.io/process/docs/FormatsAndConventions.html#commit-message)

- [programming practices - When to commit code?](https://softwareengineering.stackexchange.com/questions/83837/when-to-commit-code)
- [When is a version control commit too large?](https://softwareengineering.stackexchange.com/questions/10793/when-is-a-version-control-commit-too-large)
- [Commit only part of a file in Git](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git) this is useful to use as a cheatsheet during interactive staging
- [When do you use git rebase instead of git merge - StackOverflow](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge/804178#804178)
- [Force "git push" to overwrite remote files - StackOverflow](https://stackoverflow.com/questions/10510462/force-git-push-to-overwrite-remote-files)

- [Git Tools - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [Git Basics - Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

- [git stash - Saving Changes | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/git-stash)
- [Merging vs. Rebasing - Atlassian](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [Rewriting History - Atlassian](https://www.atlassian.com/git/tutorials/rewriting-history)

## Further Reading
- [Git for Computer Scientists](http://eagain.net/articles/git-for-computer-scientists/) - discusses the underlying implementation of `git` (merkle tree)
- [Pro Git](https://git-scm.com/book/en/v2)
- [The most useful git commands](https://orga.cat/posts/most-useful-git-commands) - a reference sheet of some handy command macros

</div>
