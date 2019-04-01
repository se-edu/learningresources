<frontmatter>
  title: Introduction to bash shell
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Linux bash shell 

Author: [Wang Junming](https://github.com/junming403)

Reviewers: [Rahul Rajesh](https://github.com/rrtheonlyone), [Ong Shu Peng](https://github.com/ongspxm), [Jeremy Choo](https://www.github.com/ChooJeremy), [Tan Zhen Yong
](https://github.com/Xenonym)

## Overview

In general, a computer can be divided into 3 abstraction layers, hardware, kernel and applications. Users cannot control the hardware directly; instructions have to be given through kernel as the kernel is the one that controls the hardware.

<img src="https://cdn-images-1.medium.com/max/1200/1*zEv6mAa2wzHnz4a5uWW4gw.png" alt="shell-kernel" style="width:300px;"/>

<sub>[What (really) happens when you type ls -l in the shell](https://medium.com/meatandmachines/what-really-happens-when-you-type-ls-l-in-the-shell-a8914950fd73)</sub>

The **shell** is the interface through which we pass instructions to the kernel. These instructions will then be executed through the hardware. In this chapter, we will look into **Linux bash shell**.

> Linux is the best-known and most-used open source operating system. As an operating system, Linux is software that sits underneath all of the other software on a computer, receiving requests from those programs and relaying these requests to the computer’s hardware. -- <cite>[What is Linux](https://opensource.com/resources/linux)</cite>

[**bash**](https://www.gnu.org/software/bash/manual/html_node/) stands for **Bourne Again SHell**, an enhanced version of the original Unix Shell program. It has become the standard shell of various linux distributions. 

## Why use command-line based shell?

A common question many people ask is: "Why should we learn the command-line based shell when there are tons of Graphical User Interface (GUI) tools out there that appear to be more user-friendly and powerful?". Well, here are some key advantages the command-line shell can give you:

- **Faster to use for remote operations**: A remote command-line shell makes it much easier to perform tasks in a cloud server, such as when a developer is connecting from home. A remote command-line shell usually requires less bandwidth, which results in less lag so that we can do things faster.

- **More stable**: No matter what linux distribution you are running, the command line shell is always available and usage is almost the same. However, GUI tools might be different in different distributions, make it very hard to learn.

- **More flexibility for power users**: You will have complete control in the command-line shell. GUIs tends to simplify things, giving the user fewer options. However, the command line always provides the full suite of options, giving you complete control.

- **More powerful in the hands of an expert user**: While you might find command-line difficult to grasp, eventual usage will familiarize yourself such that the command-line is far easier to use than trying to find options in the drop-down menus and alike of a GUI. 


### Benefit: the ability to automate tasks

One important feature in bash is the **bash script**. It enables us to do **task automation**. For example, suppose that you are a system admin and there is a task you need to perform daily. Instead of typing in the commands for the task daily, you can store these commands in the **bash script** and run it.

Here's one example of bash script, suppose you have to check if some given file exist periodically. The following script will ask for your input and output the results. `filename` is a variable used to store user input.

```bash
#!/bin/bash
 
echo -e "Please input a filename, I will check if the file exists.\n\n"

# waiting for user input.
read -p "Input a filename : " filename

# check if file exists.
test -z $filename && echo "You MUST input a filename." && exit 0
test ! -e $filename && echo "The filename '$filename' DO NOT EXIST" && exit 0

# output result.
echo "The filename: $filename is a $filetype"
```

The above bash script is still just a collection of bash commands. Although it saves us a lot of works if it is a daily routine task, it still can be done by typing those commands line by line. What makes bash script really exciting is the semantics of `conditionals`, `loops` and `functions`. Which makes bash script [**turning complete**](https://en.wikipedia.org/wiki/Turing_completeness).

The following is an example of bash script that make use of conditionals and loops. Suppose you want to let user input a directory name, check if it exists, and then output the write permission for all files inside that directory.

```bash
#!bin/bash

# read user input and check if directory exists.
read -p "Please enter a directory name: " dir
if [ "$dir" == "" -o ! -d "$dir" ]; then
	echo "The $dir is NOT exist in your system"
	exit 1
fi

# output permissions for each of the file under the directory.
filelist=$(ls $dir)
for filename in $filelist
do 
	perm=""
	test -w "$dir/$filename" && perm="readable"
	echo "The file $dir/$filename 's permission is $perm" 
done
```
**Functions** in bash script just like functions in normal programming languages, more semantics of bash script can be found [here](https://www.gnu.org/software/bash/manual/html_node/). 

bash script allows us to automate the frequently performed operations, it is easy to use and can be executed on any Unix-like operating systems without any modifications.

### Benefit: Data Stream Redirection

In linux, everything is abstracted as file, including **input**, **output**, and **error** streams. **Redirection** is a feature in Linux such that when executing a command/user program, you can change the standard input/output devices by changing its [file descriptor](https://en.wikipedia.org/wiki/File_descriptor). Each command and user program in linux is an application with input, output and errors. We can redirect the output of one application to the input of another, as such, we combined the two applications together as if we build a pipe between them.

<img src="https://www.computerhope.com/jargon/f/file-descriptor-illustration.jpg" alt="file-descriptor" style="width:200px;"/>

<sub>[File descriptor](https://www.computerhope.com/jargon/f/file-descriptor.htm)</sub>

For example, suppose you just finished your coding assignment, the resulting executable `calculate` takes a user input, do the calculation, and gives the output. To run it, you simply types:

```bash
./calculate
```

But you are not satisfied with manual testing, instead, you want to test with larger data set file `DataBundle`. And verify the output with another executable `verify`, which takes in the result as input and verify its correctness and output **PASS** or **FAIL**. Here's how we can do it.

```bash
./calculate < DataBundle > result
./verify < result
```
Notice in this case, `calculate` uses the data in `DataBundle` as input, and output the results to the file `result`. Then, `result` is taken as input to `verify`, and the final verification result is printed on screen.

Furthermore, if the code we write is buggy, it throws exception and error messages are directed to error output, then our result file will be empty! To handle it properly, we can also redirect error output to a file `errors`, and verify it is empty(such that no error occurs during execution) before we execute `verify`.

```bash
#!/bin/bash
./calculate < DataBundle > result 2> errors
if [ -s errors ]
then
     ./verify < result
else
     echo "error occured during execution."
fi
```
Another useful command is `|`, which chained linux commands together, as such the output of previous commands is passed as input to the next command.

The following command make use of shell command [ps](http://man7.org/linux/man-pages/man1/ps.1.html) and [grep](http://man7.org/linux/man-pages/man1/grep.1.html). Suppose you want to check the status of process `p` running on your system. Thus you typed in `ps aux`, and all the processes are listed, but there are too many processes that you can't find `p`. As such, you can pass the result of `(ps) aux` to `grep 'p'` to capture process `p`'s status. Thus the command will be look like `ps aux | grep 'p'`.

Input/output streams can easily be redirected, allowing information to be sent or received from files or other applications(commands and user programs). This can mean test data can be easily supplied or output captured. A more detailed introduction to I/O stream redirection can be found [here](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection).

### Benefit: Defined Standards for Help
If someone does not know a how to use certain command, they can easily see what the it provides with `man` command. There are also defined wildcard syntax standards for specifying multiple files, drawing on likely existing developer knowledge.

For example, if you are new to bash and want to learn a new command `grep`, you can simply type in `man grep` in the terminal and a help page will show up. It thoroughly include **What**, **How** as well as many other details of the command.

```
GREP(1)                   BSD General Commands Manual                  GREP(1)

NAME
     grep, egrep, fgrep, zgrep, zegrep, zfgrep -- file pattern searcher

SYNOPSIS
     grep [-abcdDEFGHhIiJLlmnOopqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
          [-e pattern] [-f file] [--binary-files=value] [--color[=when]]
          [--colour[=when]] [--context[=num]] [--label] [--line-buffered]
          [--null] [pattern] [file ...]

DESCRIPTION
     The grep utility searches any given input files, selecting lines that
     match one or more patterns.  By default, a pattern matches an input line
     if the regular expression (RE) in the pattern matches the input line
     without its trailing newline.  An empty expression matches every line.
     Each input line that matches at least one of the patterns is written to
     the standard output ......

```
## How to Get Started with linux command-line based shell?
In our opinion, there is no need to learning linux command-line based shell in particular, because there are tons of linux commands out there, it is impossible to remember and master them all at one time without actual practices. Instead, when it comes to the time that you have to use it, try to use the terminal instead of GUI based tool. Although it might be hard at first, it will benefit you more later. For example, `git`, We highly recommend get started to use command-line based git instead of visualisation tools such as `Source Tree`.

However, if you really wish to learn bash systematically, below are some resources you might find useful.


# Where to Go from Here?

- [Introduction to Linux](http://www.linfo.org/newbies.html) is a good introduction to Linux and contains many useful resource links.
- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/) is a good reference manual of linux bash shell. 
- [Online man Page](https://www.linux.org/docs/) is a collection of  `man pages`  online.
- [linux.org](https://www.linux.org/) is the Linux organization home page, you can find all relevant information about linux here. 
- Still wondering why linux could be important to your career? This [article](https://opensource.com/business/14/8/learn-linux) might worth reading.
- The [Google Shell Style Guide](https://google.github.io/styleguide/shell.xml) is a good baseline for good shell scripting style.
- Gentoo has a good [`bash` shell reference](https://devmanual.gentoo.org/tools-reference/bash/).
- Apple's [Shell Scripting Primer](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/Introduction/Introduction.html) is a in-depth introduction to shell scripting for beginners.

</div>
