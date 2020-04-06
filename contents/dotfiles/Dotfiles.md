<frontmatter>
  title: Introduction to Dotfiles
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 4
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Dotfiles

**Author(s): [Tiu Wee Han](https://github.com/tiuweehan)**<br>

<box id="article-toc">

* [Introduction to Dotfiles](#introduction-to-dotfiles)
    * [What are Dotfiles?‎](#what-are-dotfiles)
    * [Why Dotfiles?](#why-dotfiles)
        * [Benefit: Save and Reuse Configurations](#benefit-save-and-reuse-configurations)
        * [Benefit: Personalised Shortcuts and Commands‎](#benefit-personalised-shortcuts-and-commands‎)
        * [Benefit: Shell Utility and Styling](#benefit-shell-utility-and-styling)‎
    * [Getting Started](#getting-started)
        * [Management Strategies‎](#management-strategies)
    * [Conclusion](#conclusion)

</box>

### What are Dotfiles?
Dotfiles are plain text configuration files on Unix like systems (e.g. MacOS, Linux, BSD). Dotfiles store settings of almost every application, service and tool running on your system. These files control the behavior of applications from boot to termination and everything in between.

<pic src="dotfiles-logo.png" alt="Dotfiles Logo" width="45%">
  
  <sub>_Figure 1. Dotfiles Logo_ [(source)](https://www.twilio.com/blog/using-dotfiles-productivity-bootstrap-systems)</sub>

</pic>

Some common uses of dotfiles include:
- Scripts that run when a Shell is started: `.profile`, `.bashrc`, `.zshrc`
- Customising text editors: `.nanorc`, `.vimrc`, `.emacs.d`
- Storing settings for commonly used tools (e.g. `git`, `ssh`): `.gitconfig`, `.gitignore`, `.ssh/config`

### Why Dotfiles?

#### Benefit 1: Configure Applications

With Dotfiles, users have an easy and centralised way to configure their environment and applications. The usage of a file to store configurations also makes it easily shareable and reuseable by other people, as opposed to having to change settings within the application. Dotfiles can be used to configure almost all popular command line tools. In this section, we will explore dotfiles related to `git` and `ssh` - tools frequently used by developers.

##### Feature 1.1: Git Dotfiles

`git` is an indispensible tool for many developers and projects, and it is undoubtably the most popular version control software. Hence, it is highly beneficial to learn how to configure `git` dotfiles.

Global settings for `git`, such as the user's name, email and GitHub username can be specified in the `~/.gitconfig` file. The code below is a bare-bones `~/.gitconfig` file.
```git
[user]
  name = John Doe
  email = johndoe@gmail.com.

[github]
  user = johndoe123
```
Apart from just storing basic details, it can also contain other [information](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config) such as merge tools, git color scheme and aliases. This provides a centralised location for a developer to view and edit their git configurations.

Another commonly used `git` related dotfile is the `.gitignore` file, which contains a list of files and directories that a developer wants to exclude from git's version control, such as packages, binaries or secret information. Some files commonly included in `.gitgnore` include:
```git
node_modules/ # Project specific packages for node projects
.DS_Store     # MacOS file to store folder layout information in the GUI
.vs_code      # Project specific VS code text editor configuration
bin/          # Binary executables
```
This example also illustrates how using Dotfiles can add project specific configurations that all developers working on a project will adhere to universally, as opposed to system level settings which may vary from developer to developer.

##### Feature 1.2: SSH Dotfiles

For developers and engineers working with remote servers, `ssh` is arguably the most essential tool for connecting and running commands on remote servers. However, for many beginners, working with `ssh` can be a daunting prospect. Configuring dotfiles is a good way to both simplify the usage of `ssh` and learn how to master it effectively

Say a user John wants to connect to a remote server called `remoteA`. Typically, he will run the following command to connect to the server
```bash
ssh -i ~/.ssh/id_rsa john@remoteAServer.com
```

However, this is a relatively long command, and can be tedious to type especially if John accesses `remoteA` frequently. Thankfully for John, he can configure the different hosts by adding the following lines in the `~/.ssh/config` file:
```
Host remoteA
    Hostname remoteAserver.com
    User john
    IdentityFile ~/.ssh/id_rsa
```

With the configurations above, John is now able to simply just run the following to connect to `remoteA`:
```bash
ssh remoteA
```
This evidently saves time and effort needed to connect to a server. Aside from just connecting to servers, `ssh` can also be used for other purposes, all of which are easily [configurable](https://www.ssh.com/ssh/config) using dotfiles. These include [SSH proxies](https://www.cyberciti.biz/faq/linux-unix-ssh-proxycommand-passing-through-one-host-gateway-server/), [launching GUI applications remotely](https://www.nics.tennessee.edu/x11_forwarding) and [SSH forwarding](https://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/).

<box type="warning">
Please ensure that your RSA private keys such as `~/.ssh/id_rsa` are kept safe at all times.
</box>

#### Benefit 2: Personalised Shortcuts and Commands

Dotfiles allow users to create custom shortcuts and commands, including aliases, functions and key-bindings, that can tremendously improve productivity. These can be both system level and project specific. In this section, we will look into aliases and functions.

##### Feature 2.1: Aliases

An alias is a short cut command to a longer command. A new alias is defined by assigning a string with the command to a name in the format `alias <name>=<command>`. Aliases are often set in the `~/.bashrc` or `~/.zshrc` file. The examples below illustrate the use of aliasing.

```bash
alias c='clear'
alias lf='/bin/ls --color -CF'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias r='fc -s'
alias vi='vim'
```

In this case, a user is able to type `c` instead of typing out `clear` in order to clear the terminal screen, a very commonly used command. Aliases are especially useful for abstracting long commands that are used often, and when compounded can save the user a lot of time and effort.

##### Feature 2.2: Functions

Much like aliases, functions allows a user to abstract longer commands into short commands. However, there is additional functionality such as being able to pass command line [arguments](https://tecadmin.net/tutorial/bash-scripting/bash-command-arguments/) and [flags](https://stackoverflow.com/questions/192249/how-do-i-parse-command-line-arguments-in-bash). This makes the commands more extensible and reuseable for a wide array of functionality.

The example function `say_hello` below takes in 2 arguments, represented by `$1` and `$2`.
```bash
say_hello () {
  echo "hello $1, good $2!"
}
```
If the user were to type the following into the terminal:
```bash
say_hello John morning
```
it would produce the output:
```
hello John, good morning!
```

The use of functions essentially allows a user to create custom command-line tools to suit their needs.
Defining the function above in `~/.bashrc` or `./.zshrc` will load it whenever a new shell is created.

#### Benefit 3: Shell Styling and Utility

Dotfiles are also used to style shells (e.g. `bash`, `zsh`, `fish`) and enhance or add features to them. The styling of shells not only improves the overall aesthetic, it also makes them more readable (shell output can be tedious to read and digest). Features such as adding system information to the prompt, tab completion and syntax highlighting also improves the overall utility and feedback of the shell, contributing to a better developer experience.

##### Feature 3.1: Prompt

The `PS1` environment variable provides information for configuring and styling the Shell prompt. 
By default, `PS1` is set to display only the username, host name and current working directory in the prompt.

<pic src="default-prompt.png" alt="Default prompt" width="55%">

  <sub> _Figure 2. Default prompt_ </sub>

</pic>

`PS1` can be [configured](https://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html) to add (or remove) features, such as status code of the previous command, git branch, git status, dates, etc. In Figure 3 below, the prompt is able to detect and show the git branch and status when it is inside a git repository, a useful and convenient feature for developers.

<pic src="enhanced-prompt.png" alt="Enhanced prompt" width="55%">

  <sub> _Figure 3. Enhanced prompt_ </sub>

</pic>

##### Feature 3.2: Tab Completion

Tab completion is a feature that allows the user to select options from a drop down menu. This is a default feature when using `zsh`, but can be configured using dotfiles when using other shells like `bash`.

<pic src="cd-autocomplete.gif" alt="Git flag autocomplete" width="60%">

  <sub> _Figure 4. `cd` tab completion [(Tutorial)](https://scriptingosx.com/2019/07/moving-to-zsh-part-5-completions/)_ </sub>

</pic>

Tab completion can also be used for other purposes, such as selecting flags from a drop down menu, complete with an accompanying description.

<pic src="git-flag-autocomplete.gif" alt="Git flag autocomplete" width="60%">

  <sub> _Figure 5. Flag autocomplete_ </sub>

</pic>

##### Feature 3.3: Syntax Highlighting

Dotfiles are also often used to style and colorise the terminal. This is not just for aesthetic reasons - the use of color enhances readability of programs and makes it easier to debug. One such example would be the use of **syntax highlighting** as seen in Figure 6 below. It is especially useful for long shell commands, escape sequences or string interpolation.

<pic src="syntax-highlighting.gif" alt="Git flag autocomplete" width="60%">

  <sub> _Figure 6. Syntax Highlighting [(Tutorial)](https://github.com/zsh-users/zsh-syntax-highlighting)_ </sub>

</pic>

### Getting Started

While dotfiles are easy to setup and configure, they can become disorganised over time as a developer as more and more configurations are added. As much as possible, they should be managed like any other software engineering project, applying software design principles such as modularisation to make them extensible and future-proof.

#### Management Strategies

There are many stratgies for managing dotfiles, but virtually all of them revolve around storing them in `git` repositories. The nature of dotfiles make `git` and ideal management tool - Dotfiles continously evolve over time as the user adds more configurations, and it may be useful to track old dotfiles for future reference.

Another major benefit of managing dotfiles with `git` is that they can then be pushed to online repositories like GitHub or GitLab. These dotfiles can then be easily reused on other systems by simply pulling from these online repositories, allowing users to port their painstakingly created dotfiles anywhere. Since certain dotfiles may also vary across different systems (e.g. bash configurations may be different), users can use different branches to differentiate between these systems. It almost seems as though `git` was created for managing dotfiles!

The main difference between different strategies are how these dotfiles should be linked from the git repository into the system, since dotfiles can exist in different directories. The 3 most commonly used strategies are detailed below:

1. **Use a [git worktree](https://www.atlassian.com/git/tutorials/dotfiles):**\
The strategy uses a less well-known Git functionality of storing the Git worktree separately from the Git directory. This consists of a Git bare repository in a "side" folder (like `$HOME/.cfg` or `$HOME/.myconfig`) using a specially crafted alias so that commands are run against that repository and not the usual .git local folder, which would interfere with any other Git repositories around.
2. **Use [symlinking](https://opensource.com/article/19/3/move-your-dotfiles-version-control):**\
This strategy involves using symlinks (The `ln` command) to create a shortcut linking to the dotfiles repository. The `ln` command is used to create links in your (unix-based) system. The syntax is `ln -s <actual location of the file> <name and location you want to see that file under>`. For example, the command below will result in the gitconfig in the dotfiles directory to be accessible from the `~/.gitconfig` location, which is where Git is expecting to see all the Git preferences set.

```bash
ln -s ~/dotfiles/gitconfig ~/.gitconfig
```
3. **Use an existing dotfiles management tool such as [yadm](https://yadm.io/):**
`yadm` is a command-line tool for managing dotfiles. It provides many features out of the box, which saves the user the time needed to manually configure his/her own dotfiles. It also supplies additional features, such as the ability to manage a subset of secure files, which are encrypted before they are included in the repository. However, the caveat of this strategy is that it requires new systems to have `yadm` installed, which significantly reduces the portability.

<box type="warning">
Before copying dotfiles over to a system, ensure that there is a backup of the local dotfiles so they are not overwritten.
</box>

### Conclusion

In a nutshell, Dotfiles are highly useful tools that can provide virtually unlimited customisability, and can tremendously improve the productivity of a developer. For junior developers that are keen on improve their linux or shell scripting knowledge, dotfiles are a good way to get started.

Apart from those listed in the article, here are some further readings/resources to get started with Dotfiles:

- [Unofficial Guide to Dotfiles on GitHub](https://dotfiles.github.io/)
- [Introduction to Dotfiles](https://thoughtbot.com/upcase/videos/intro-to-dotfiles)
- [Awesome dotfiles: A curated list of dotfiles resources.](https://github.com/webpro/awesome-dotfiles)

</div>
