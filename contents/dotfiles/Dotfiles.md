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

### What are Dotfiles?
Dotfiles are plain text configuration files on Unix like systems (e.g. MacOS, Linux, BSD). Dotfiles store settings of almost every application, service and tool running on your system. These files control the behavior of applications from boot to termination and everything in between.

<pic src="dotfiles-logo.png" alt="Dotfiles Logo" width="45%">
  
  <sub>_Figure 1. Dotfiles Logo_ [(source)](https://www.twilio.com/blog/using-dotfiles-productivity-bootstrap-systems)</sub>

</pic>

Some common examples of dotfiles include:
- `.profile`, `.bashrc`, `.zshrc`: scripts that run when a Shell is started. 
- `.nanorc`, `.vimrc`, `.emacs.d`: dotfiles for configuring text editors like nano, vim and emacs respectively.
- `.gitconfig`, `.gitignore`: dotfiles containing git configurations.
- `.ssh/config`: dotfiles containing SSH configurations.

### Why Dotfiles?
1. **Dotfiles improve productivity.** Dotfiles allow users to tailor their environments to their needs and preferences, from general purpose configurations to project-specific commands. There are also many widely available libraries and plugins to choose and match, providing the most optimal tools to improve their productivity.

1. **Feel at `$HOME` Wherever You Go.** For developers who have customised their local environments to their personal tastes, there is no place as good as `$HOME`. With proper dotfiles management, it is possible to easily port these customisations to any system, and remove them just as easily.

1. **Improve Linux skills.** Setting up dotfiles is an enriching experience that will enable developers to improve command of various skills such as shell scripting and linux tools. Configuring application related tools such as `git` and `ssh` is also a good opportunity to familiarise and become experts with using these tools.

#### Benefit: Save and Reuse Configurations

Dotfiles provide users with an easy and centralised way to configure their environment and applications. Dotfiles are used to configure almost all popular command line tools. In this section, we will explore dotfiles related to `git` and `ssh` - tools frequently used by developers.

##### Feature: Git Dotfiles

`git` is an indispensible tool for many developers and projects, and it is undoubtably the most popular version control software. Hence, it is highly beneficial to learn how to configure `git` dotfiles.

Global settings for `git`, such as the user's name, email and GitHub username can be specified in the `~/.gitconfig` file. The code below is a bare-bones `~/.gitconfig` file.
```git
[user]
  name = John Doe
  email = johndoe@gmail.com.

[github]
  user = johndoe123
```
Apart from just storing basic details, it can also contain other [information](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config) such as merge tools, git color scheme and aliases.

Another commonly used `git` related dotfile is the `.gitignore` file, which contains a list of files and directories that a developer wants to exclude from git's version control, such as packages, binaries or secret information. Some files commonly included in `.gitgnore` include:
```git
node_modules/ # Project specific packages for node projects
.DS_Store     # MacOS file to store folder layout information in the GUI
.vs_code      # Project specific VS code text editor configuration
bin/          # Binary executables
```

##### Feature: SSH Dotfiles

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

<box type="danger">
Please ensure that your RSA private keys such as `~/.ssh/id_rsa` are kept safe at all times.
</box>

#### Benefit: Personalised Shortcuts and Commands

A common usage of dotfiles is to create personalised shortcuts and commands on the shell. These are achieved through the use of aliases and functions.

##### Feature: Aliases

An alias is a short cut command to a longer command. A new alias is defined by assigning a string with the command to a name in the format `alias <name>=<command>`. Aliases are often set in the `~/.bashrc` or `~/.zshrc` file. The examples below illustrate the use of aliasing.

```bash
alias c='clear'
alias lf='/bin/ls --color -CF'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias r='fc -s'
alias vi='vim'
```

In this case, a user is able to type `c` instead of typing out `clear` in order to clear the terminal screen, a very commonly used command. Aliases are especially useful for abstracting long commands that are used often.

##### Feature: Functions

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

#### Benefit: Shell Utility and Styling

Dotfiles are also often used to enhance or add features to a shell, such as `bash` or `zsh`.

##### Feature: Prompt

The `PS1` environment variable provides information for configuring and styling the Shell prompt. 
By default, `PS1` is set to display only the username, host name and current working directory in the prompt.

<pic src="default-prompt.png" alt="Default prompt" width="55%">

  <sub> _Figure 2. Default prompt_ </sub>

</pic>

`PS1` can be [configured](https://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html) to add (or remove) features, such as status code of the previous command, git branch, git status, dates, etc. In Figure 3 below, the prompt is able to detect and show the git branch and status when it is inside a git repository, a useful and convenient feature for developers.

<pic src="enhanced-prompt.png" alt="Enhanced prompt" width="55%">

  <sub> _Figure 3. Enhanced prompt_ </sub>

</pic>

##### Feature: Tab completion

Tab completion is a feature that allows the user to select options from a drop down menu. This is a default feature when using `zsh`, but can be configured using dotfiles when using other shells like `bash`.

<pic src="cd-autocomplete.gif" alt="Git flag autocomplete" width="60%">

  <sub> _Figure 4. `cd` tab completion [(Tutorial)](https://scriptingosx.com/2019/07/moving-to-zsh-part-5-completions/)_ </sub>

</pic>

Tab completion can also be used for other purposes, such as selecting flags from a drop down menu, complete with an accompanying description.

<pic src="git-flag-autocomplete.gif" alt="Git flag autocomplete" width="60%">

  <sub> _Figure 5. Flag autocomplete_ </sub>

</pic>

##### Feature: Syntax Highlighting

Dotfiles are also often used to style and colorise the terminal. This is not just for aesthetic reasons - the use of color enhances readability of programs and makes it easier to debug. One such example would be the use of **syntax highlighting** as seen in Figure 6 below. It is especially useful for long shell commands, escape sequences or string interpolation.

<pic src="syntax-highlighting.gif" alt="Git flag autocomplete" width="60%">

  <sub> _Figure 6 Syntax Highlighting [(Tutorial)](https://github.com/zsh-users/zsh-syntax-highlighting)_ </sub>

</pic>

### How to get Started with Dotfiles?

While dotfiles are easy to setup and configure, they can become disorganised over time as a developer as more and more configurations are added. As much as possible, they should be managed like any other software engineering project, applying software design principles such as modularisation to make them extensible and future-proof.

#### Management Strategies

There are several management stratgies for managing dotfiles, and most of them revolve around `git` configurations. The benefit of managing dotfiles with `git` is that they can then be pushed to online repositories like `GitHub` or `GitLab`, which can .

1. **Use a [git worktree](https://www.atlassian.com/git/tutorials/dotfiles):**\
The technique consists in storing a Git bare repository in a "side" folder (like `$HOME/.cfg` or `$HOME/.myconfig`) using a specially crafted alias so that commands are run against that repository and not the usual .git local folder, which would interfere with any other Git repositories around.
2. **Use [symlinking](https://opensource.com/article/19/3/move-your-dotfiles-version-control):**\
The `ln` command is used to create links in your (unix-based) system. Use the `--help` command to get more info on the myriad of options available to you. Just using the `ln` command creates something called a ‘hard link’, which is not what we want. We want a ‘symbolic’ link instead. The syntax is `ln -s <actual location of the file> <name and location you want to see that file under>`. For example:
```bash
ln -s ~/dotfiles/gitconfig ~/.gitconfig
```
will result in the gitconfig in the dotfiles directory to be accessible from the `~/.gitconfig` location, which is where Git is expecting to see all the Git preferences set.

3. **Use an existing dotfiles management tool such as [yadm](https://yadm.io/):**
`yadm` is a command-line tool for managing dotfiles. with additional features such as allowing developers to select alternate versions of files based on the operating system or host name. `yadm` also supplies the ability to manage a subset of secure files, which are encrypted before they are  included in the repository.

<box type="danger">
Before copying dotfiles over to a system, ensure that there is a backup of the local dotfiles so they are not overwritten.
</box>

### Conclusion

In a nutshell, Dotfiles are highly useful tools that can provide virtually unlimited customisability, and can tremendously improve the productivity of a developer. For junior developers that are keen on improve their linux or shell scripting knowledge, dotfiles are a good way to get started.

Apart from those listed in the article, here are some further readings/resources to get started with Dotfiles:

- [Unofficial Guide to Dotfiles on GitHub](https://dotfiles.github.io/)
- [Introduction to Dotfiles](https://thoughtbot.com/upcase/videos/intro-to-dotfiles)
- [Awesome dotfiles: A curated list of dotfiles resources.](https://github.com/webpro/awesome-dotfiles)
