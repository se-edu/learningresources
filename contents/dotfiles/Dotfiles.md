<frontmatter>
  title: Introduction to Dotfiles
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Dotfiles

**Author(s): [Tiu Wee Han](https://github.com/tiuweehan)**<br>

<box id="article-toc">

* [Introduction to Dotfiles‎](#introduction-to-dotfiles)
    * [What are Dotfiles?‎](#what-are-dotfiles)
    * [Why Manage Dotfiles?](#why-manage-dotfiles)
    * [How to Get Started with Dotfiles?‎](#how-to-get-started-with-dotfiles)
        * [Where to Go from Here?‎](#where-to-go-from-here)
</box>

### What are Dotfiles?
Dotfiles are plain text configuration files on Unix like systems. Dotfiles store settings of almost every application, service and tool running on your system. These files control the behavior of applications from boot to termination and everything in between.

<pic src="dotfiles-logo.png" alt="Dotfiles Logo" width="45%">
  
  <sub>_Figure 1. Dotfiles Logo_ [(source)](https://www.twilio.com/blog/using-dotfiles-productivity-bootstrap-systems)</sub>

</pic>

Some examples of dotfiles include:
- `.profile`, `.bashrc`, `.zshrc`: dotfiles containing scripts that run when a Shell is started. 
- `.nanorc`, `.vimrc`, `.emacs.d`: dotfiles for text editors like nano, vim and emacs respectively.
- `.gitconfig`, `.gitignore`: dotfiles containing git configurations.
- `.ssh/config`: dotfiles containing SSH configurationsk

### Why Dotfiles?
1. **Dotfiles improve productivity.** For example, long repetitive commands

1. **Feel at `$HOME` Wherever You Go.** For developers who have customised their local environments to their personal tastes, there is no place as good as `$HOME`. With proper dotfiles management, it is possible to easily port these customisations to any system, and remove these customisations just the same.

1. **Improve Linux skills.** Setting up dotfiles is an enriching experience that will enable you to improve command of various skills such, such as shell scripting, and improve familiarity with command line tools such as `git` and `ssh`.

#### Feature: Personalised Shortcuts and Commands

A common usage of dotfiles is to create personalised shortcuts and commands on the shell. These are achieved through the use of aliases and functions.

##### Aliases

An alias is a short cut command to a longer command. Users may type the alias name to run the longer command with less typing. A new alias is defined by assigning a string with the command to a name. Alias are often set in the `~/.bashrc` or `~/.zshrc` file.

```bash
alias c='clear'
alias lf='/bin/ls --color -CF'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias r='fc -s'
alias vi='vim'
```

For the examples above

##### Functions

Much like aliases, functions allows a user to abstract longer commands into short commands. However, there is additional functionality such as being able to pass arguments and flags. This solves the makes scripts more readable and to avoid writing the same code over and over again.

```bash
function_name () {
  commands
}
```

#### Feature: Customised Shell Styling



##### Prompt

#### Feature: Save and Reuse Configurations

##### Git Configurations

##### SSH Configurations

#### A Word of Caution

### How to get Started with Dotfiles?

#### Management Strategies

There are several management stratgies for managing dotfiles, and most of them revolve around `git` configurations.

1. Use a git worktree.
1. Use symlinking
1. Use an existing dotfiles management tool.

### Conclusion