# Bandit

[Bandit](https://overthewire.org/wargames/bandit/) is wargame offered by [OverTheWire Community](https://overthewire.org/). It introduces and teaches about various linux commands.

The following is my write-up for levels in this wargame. Note that I don't show passwords but only the solution for the level.

## Level 0

### Level Goal

>The goal of this level is for you to **log into the game using SSH**. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

### Solution

```shell
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
...
bandit0@bandit.labs.overthewire.org's password:(type bandit0)
```

### Notes

- `Shift + Insert` can be used to paste in terminal. See [How to paste into a terminal?](https://stackoverflow.com/questions/7123709/how-to-paste-into-a-terminal)

- `Ctrl + Right Arrow` or  `Ctrl + Left Arrow` can be used to jump through text.

### Links for Learning More

- Tutorials
    - [What Is SSH: Understanding Encryption, Ports and Connection](https://www.hostinger.com/tutorials/ssh-tutorial-how-does-ssh-work)

    - [SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)

- Readings
    - [Secure Shell Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)
    
    - [OpenSSH Specifications](https://www.openssh.com/specs.html)
    
- Others
    - [OpenSSH website](https://www.openssh.com/)

    - [ssh man page](https://man7.org/linux/man-pages/man1/ssh.1.html)

## Level 0 -> 1

### Level Goal

>The password for the next level is stored in a **file called readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### Solution

```shell
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
...
bandit0@bandit.labs.overthewire.org's password:(type bandit0)
...
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme 
```

Then, it should give out the password for next level.

### Notes

- We can see from first line of `help` command that the machine is using [GNU bash](https://www.gnu.org/software/bash/).
```shell
bandit0@bandit:~$ help
GNU bash, version 5.1.16(1)-release (x86_64-pc-linux-gnu)
...
```
So [GNU Bash manual](https://www.gnu.org/software/bash/manual/) might come in handy.

- [man](https://man7.org/linux/man-pages/man1/man.1.html) command can be used to read [man pages](https://en.wikipedia.org/wiki/Man_page) for commands. For example `man ssh`. You can also use [Linux man pages online](https://man7.org/linux/man-pages/index.html) if you like reading web pages. This is useful for learning about unfamiliar commands.

- `cat` command reads from *standard input* if no argument is specified.
```shell
bandit0@bandit:~$ cat
Hello
Hello
asdf
asdf
```

### Links for Learning More

- Tutorials
    - [The Linux commands line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)

    - [Bash Scripting Tutorial – Linux Shell Script and Command Line for Beginners](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/)

- Readings
    - [Standard streams on wikipedia](https://en.wikipedia.org/wiki/Standard_streams)

    - [Understanding standard input, standard output, and standard error](https://www.ibm.com/docs/en/zos/2.2.0?topic=commands-understanding-standard-input-standard-output-standard-error)

- Others
    - [ls man page](https://man7.org/linux/man-pages/man1/ls.1.html)

    - [cat man page](https://man7.org/linux/man-pages/man1/cat.1.html)

## Level 1 -> 2

### Level Goal

>The password for the next level is stored in a file called **-** located in the home directory.

### Solution

```shell
$ ssh bandit1@bandit.labs.overthewire.org -p 2220
...
bandit1@bandit.labs.overthewire.org's password:(type the password we got from previous level)
...
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat -
ads
ads
Hello
Hello
^C
bandit1@bandit:~$ 
```

We can use `Ctrl + C` to terminate active program.

It seems like it is echoing back whatever input we are giving. Naming a file a dash '-' is not very ordinary. It should mean something special.

Let's see if `man cat` has to say something about it. And, sure enough it has.

>With no FILE, or **when FILE is -, read standard input**.

That explains the behavior from before. It's just reading everything we enter(standard input) and outputting them back. But, there are multiple ways to solve this problem.

#### Method 1 (Other commands)
There are many commands that read files but do not use - for standard input. We can use them for reading out the password. The following are list some commands that can help us.

* #### [more](https://man7.org/linux/man-pages/man1/more.1.html)
```shell
bandit1@bandit:~$ more -
```
This should print out the password.

* #### [rev](https://man7.org/linux/man-pages/man1/rev.1.html)
```shell
bandit1@bandit:~$ rev -
```
It will print out the reversed password. So, you have to reverse again to get the password.

* #### [hexdump](https://man7.org/linux/man-pages/man1/hexdump.1.html)
```shell
bandit1@bandit:~$ hexdump - -C
```
The password should be printed along side with the its hex representation.

#### Method 2 (Cat)
We can still use cat command for reading the password. There are two ways to do this.

First way is to use redirection in bash. See [Section 3.6.1](https://www.gnu.org/software/bash/manual/html_node/Redirections.html) for the following solution.
```shell
bandit1@bandit:~$ cat < -
```
When the above is executed, - is opened for reading on file descriptor 0(standard input). Then, the cat command reads input from standard input which is now associated with the file -. So, the password is read and printed out.

Second way is to use absolute path. The `pwd` command can tell use which directory currently we are.
```shell
bandit1@bandit:~$ pwd
/home/bandit1
```
So, the file - must be at /home/bandit1/- in the system.
```shell
bandit1@bandit:~$ cat /home/bandit1/-
```
Or you can use dot folder '.' The dot folder represents the current directory(in our case `/home/bandit1`) while double dot '..' folder represents the parent directory(`/home/`)
```shell
bandit1@bandit:~$ cat ./-
```

#### Method 3 (Python)
[Python](https://www.python.org/) interpreter is commonly included in many linux machines. So, we should check if we have one.
```shell
bandit1@bandit:~$ python3 --version
Python 3.10.12
``` 

It indeed has it. Python can be used to read or write files. See [Reading and Writing Files](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files).
```shell
bandit1@bandit:~$ python3
Python 3.10.12 (main, Jun 11 2023, 05:26:28) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> f = open('-','r')
>>> f.read()
```
It should print out the password inside python interpreter. Be sure to remove the trailing new line('\n') at the end.

### Links for Learning More

- Learning Python
    - [Python For Beginners](https://www.python.org/about/gettingstarted/)

    - [The Python Tutorial](https://docs.python.org/3/tutorial/index.html)

- Readings
    - [Dot Definition](https://www.linfo.org/dot.html)
    
    - [Filesystem Hierarchy Standard on wikipedia](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

- Tools
    - [explainshell.com](https://explainshell.com/) is a useful tool for learning unfamiliar shell expressions. For example [explanation of our redirection solution](https://explainshell.com/explain?cmd=cat+%3C+-).

## Level 2 -> 3

### Level Goal

>The password for the next level is stored in a file called **spaces in this filename** located in the home directory

### Solution
```shell
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces in this filename
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```
It seems like `cat` sees `spaces in this filename` as separated arguments and not a single argument. We can get around this by using `\` to escape the space after each words. See [Escape Character](https://www.gnu.org/software/bash/manual/bash.html#Escape-Character).
```shell
bandit2@bandit:~$ cat spaces\ in\ this\ filename
```
This should treat `spaces in this filename` as one argument for `cat` command and print out the password.

Or, we can use single quotes (') or double quotes (") to achieve the same idea. See [Single Quotes](https://www.gnu.org/software/bash/manual/bash.html#Single-Quotes) and [Double Quotes](https://www.gnu.org/software/bash/manual/bash.html#Double-Quotes) from bash manual.
```shell
bandit2@bandit:~$ cat "spaces in this filename"
[or]
bandit2@bandit:~$ cat 'spaces in this filename'
```
Both should print out the password.

### Notes

- If you want to use a single quote inside enclosed single quotes (eg. `echo 'It's working'`) you can escape the single quote with non-quoted backslash `\` (eg. `echo 'It\'s working'`). The same idea applies for double quotes and the backslash itself (eg `echo backslash looks like \\.`).

### Links for Learning More

- Readings
    - [Escape character on wikipedia](https://en.wikipedia.org/wiki/Escape_character)

    - [Escape sequence on wikipedia](https://en.wikipedia.org/wiki/Escape_sequence)


## Level 3 -> 4

### Level Goal

>The password for the next level is stored in **a hidden file** in the inhere directory.

### Solution
When reading manual of `ls`, we can see this option.

>-a, --all
>   do not ignore entries starting with .

So, without `-a`, `ls` is ignoring files starting with dot. This information should be enough to solve the level.

### Links for Learning More
- Readings
    - [Hidden file and hidden directory on wikipedia](https://en.wikipedia.org/wiki/Hidden_file_and_hidden_directory)

## Level 4 -> 5

### Level Goal

>The password for the next level is **stored in the only human-readable file** in the inhere directory.

### Solution
```shell
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ cat -file00
cat: invalid option -- 'f'
```
`cat` is treating every arguments starting with - as option. We can get around this by using the trick from before.
```shell
bandit4@bandit:~/inhere$ cat ./-file00
QRrtZ�i�        �H
                  |��ȧ����^��bandit4@bandit:~/inhere$
```
It reads out the file -file00 but the content is clearly unreadable for human. It seems like we have to `cat` out every files from 00 to 09 manually.

But, there is a faster way to do it. We can use [Pattern Matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html) feature from bash.
```shell
bandit4@bandit:~/inhere$ cat ./*
QRrtZ�i�        �H
                  |��ȧ����^��7L3��Y�ͯ   Ŵ����E�Y�ܚ      �V&��h�F���y���O̫��`�\�-⃐�Hx��2��K��i�x�#e�>�␦VO��p{�   ���MUb4�␦��gQ��eE}:�g���j8�����<.�e��S��e 0�����]7�������b�<�~G=1���␦����B׃�"
                                 ���W��9ؽ5lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
���K�~�+��9"T���*Z$���"�r�
�Z�\�������ж�q���7����/�n��nbandit4@bandit:~/inhere$
```
`*` means matches any [string](https://en.wikipedia.org/wiki/String_(computer_science)). So, it goes through every files in the current directory and prints out their content. We can somewhat see some human readable characters. But, this is still messy.

[strings](https://man7.org/linux/man-pages/man1/strings.1.html) is a command for displaying printable strings in a file. We can use it for filtering out our password.
```shell
bandit4@bandit:~/inhere$ strings ./*
```
This should display our password along with other readable characters from other files.

### Links for Learning More

- Readings
    - [Regular expression from wikipedia](https://en.wikipedia.org/wiki/Regular_expression)

## Level 5 -> 6

### Level Goal

>The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
><br> **human-readable**
><br> **1033 bytes in size**
><br> **not executable**

### Solution
`ls` command has option `-l` called long listing format that includes both file permission and file size in display. 

Pipe '|' can be used to redirect output of one command to another command input (eg `echo Hello | cat`). 

[grep](https://man7.org/linux/man-pages/man1/grep.1.html) is a command that display lines in a file that matches patterns. Like `cat` it reads from standard input if no filename is specified.

Example:
```shell
$ cat test.txt
Hello World
Hi
$ grep "World" test.txt
Hello World
$ printf "Hello\nWorld" | grep "ll"
Hello
```
These information should be enough to solve our challenge.
```shell
bandit5@bandit:~/inhere$ ls -la ./maybehere* | grep "1033"
-rw-r-----  1 root bandit5 1033 Oct  5 06:19 .file2
```
Surely, we find the file with 1033 bytes and size and it is not executable ('x' is not present in file permission). But, `grep` prints out only lines containing "1033" and directory name which has our .file2 file is lost.

We can check `man grep` if it has some options for printing out the lines before the match. And, sure it has.
>-B NUM, --before-context=NUM<br>
>Print  NUM  lines  of leading context before matching lines.  Places a line containing a group separator (--) between contiguous groups of matches.  With the -o or --only-matching option, this has no effect and a warning is given.

This information should be enough to finish the level.


### Links for Learning More

- Readings
    - [File-system permissions on wikipedia](https://en.wikipedia.org/wiki/File-system_permissions)

    - [An Introduction to Linux Permissions](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-permissions)

    - [Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)

- Tutorials
    - [File Permissions in Linux – How to Use the chmod Command](https://www.freecodecamp.org/news/file-permissions-in-linux-chmod-command-explained/)

    - [Working with pipes on the Linux command line](https://www.redhat.com/sysadmin/pipes-command-line-linux)