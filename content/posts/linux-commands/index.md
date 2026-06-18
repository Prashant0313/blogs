+++
date = '2026-06-03T20:11:01+05:30'
draft = false
title = 'linux commands'
+++

# linux fundamentals from `scratch`…

## 1. Opening and Closing the Terminal

open : `Ctrl+alt+t`
close: `Ctrl+d`

## 2. Basic Commands

`echo` : returns command line arguments to standard output.
`date` : show the current date and time
`cal` : show the calendar
`cat` : read and write files
`cut` : divided into blocks and gives u fields
`grep` : grep the content from the file
`cd` : change directories
`ls` : list files and directories
`man` : man command is use for finding man pages.
`apropos`: to finding man pages or commands.

## 3. Command History

`history`
`!!` : to run previous command
`!n` : to run command from history using index number

## 4. Some Important Definitions

**command** : an instruction typed in the terminal and submitted to the shell for interpretation and a command must be a valid program on shell's path.e.g. `cal` , `cat` etc
**shell** : a program that interprets commands for meaning (arthaart).
**terminal** : a graphical window where command can be typed and submitted to the shell

## 5. Command Structure 
`command_Name -option(--option) argumentscommand`

**5.1 Command name** :- command_Name must be a valid program on shell’s path. to check the `which` command is it.
**5.2 Options** : - to customize the commands behavior
5.2.1 Short-form Options : -option
5.2.2 Long-form Options : - -option
5.2.3 Command Line Arguments : args
5.2.4 Arguments for Options : command line arguments are type of input that commands
```
operate on e.g. `cal` 12 2019, `echo` "hello"
```

## 6. Using the Manual
`man` _k_ “search” it is help to search the programs in systems and it provide the whole info about the command

| Section Number | Section Title | Description |
| :---: | :--- | :--- |
| **1** | **User Commands** | Commands that can be run from the shell by a normal user (typically no administrative privileges are needed). |
| **2** | **System Calls** | Programming functions used to make calls to the Linux Kernel. |
| **3** | **C Library Functions** | Programming functions that provide interfaces to specific programming libraries. |
| **4** | **Devices and Special Files** | File system nodes that represent hardware devices or software devices. |
| **5** | **File Formats and conventions** | The structure and format of file types or specific configuration files. |
| **6** | **Games** | Games available on the system. |
| **7** | **Miscellaneous** | Overviews of miscellaneous topics such as protocols, filesystems, and so on. |
| **8** | **System administration tools and daemons** | Commands that require root or other administrative privileges to use. |

## 7. find

This use search files and directories in system by there name, size, permission and you also execute commands in it. this is not as much fast compare to locate command. this work as `ls` command works but recursively.

`find` [options]… …

## 8. Command Input and Output
Standard Data Streams can be redirected and are identified using their stream number.

8.1 Redirecting Standard Output: `1>` or `>` destination
8.2 Redirecting Standard Error: `2>` destination
8.3 Redirecting Standard Input: `0<` or `<` destination **9.** **Piping :** Redirection of the standard output of one command to the standard input of another command is known as piping. it is represented by pip symbol (`|`)
e.g. `commandOne –options arguments | commandTwo –options arguments`

## 9 Taking “Snapshots” of pipeline data using the tee command

Redirecting during a pipeline breaks the pipeline.For example, this wouldn’t work: `commandOne –options arguments > snapshot.txt | commandTwo –options arguments` Because redirection is processed by the shell before piping is, _snapshot.txt_ would be created,but this locks up the standard output stream and therefore no data can be passed through the pipeline to `commandTwo`. NB: Redirection breaks pipelines However, the `tee` command allows us to take a “snapshot” of the data in the pipeline without breaking the pipeline. `commandOne –options arguments | tee snapshot.txt | commandTwo –options arguments` Here, a snapshot of the data coming out of `commandOne` is saved in _snapshot.txt_, but the data is also successfully piped through to `commandTwo`.

e.g. `date` | `tee` /home/user/date.txt | `cut` -d " " -f 1 > /home/user/today.txt

 **Piping to commands that only accept command line arguments by using xargs**

Piping connects the standard output of one command to the standard input of another command. But what if the second command doesn’t accept standard input? e.g. the echo command. The key is to transform the data coming in, into command line arguments. This is possible using the xargs command. For example, this would not work: `commandOne –options arguments | echo` This would work: `commandOne –options arguments | xargs echo`

e.g. `date | tee /home/user/date.txt | cut -d " " -f 1 | tee /home/user/today.txt | xargs echo "today is "`

## 10. Aliases : 
Aliases allow you to save your pipelines and commands with easy to remember nicknames so that they can be used later much easier.

You define aliases in your _.bash_aliases_ file in your home directory. if you don’t have it in your home directory so should make it.

Here is how you define an alias in .bash_aliases: alias aliasName=”THING YOU WANT TO ALIAS”

e.g. alias mojkrdi=‘`date | tee /home/user/date.txt | cut -d " " -f 1 | tee /home/user/today.txt | xargs echo "today is "`’

## 11. file system


```
/       : The Very Top (Root) of The File Tree. Holds Everything else.

/bin    : Stores Common Linux user command binaries. e.g date, cat, cal commands 
	  are in here.

/boot   : Bootable linux Kernel and bootloader config files

/dev    : Files representing devices. tty=terminal, fd=floppydisk, 
          (sd or hd) = harddiss, ram=RAM, cd=CD-ROM

/etc    : Administrative Configuration files. The format for many of these 
	  configuration can be found in section 5 of the Linux Manual.

/home   : Where the home directories for regular users are stored. For example, 
	  mine is at /home/kali

/media  : Unlike /dev, /media is usually where removable media (USB sticks, 
	  external hard drives etc.) are mounted.

/lib     : Contains shared libraries needed by applications in /bin and /sbin to 
	   boot the system.

/mnt    : A place to mount external devices. This can still be used but has been 
          superseded by /media

/misc   : A directory used to sometimes automount filesystems on request.

/opt    : Directory Structure used to store additional (i.e optional) software

/proc   : Information about System Resources

/root   : The home folder for the root user aka the superuser (similar to the 
          administrator on Windows)

/sbin   : Contains administrative commands (binaries) for the root (super) user.

/tmp    : Contains temporary files used by running applications.

/usr    : Contains files pertaining to users that in theory don’t change after 
          installation.

/var    : Contains directories of variable data that could be used by various 
          applications. System log files are usually found here.
```

## 12. rules & convention of naming files and folder in Linux

1. All files name are case sensitive
2. You can use upper and lowercase letters, numbers, “.” (dot), and “_” (underscore) symbols and blank spaces but avoid to use blank spaces in linux because it will create huge mess when you working at command line interface. yeah its a nightmare in linux don’t do that.
3. No need you extension in file name but it is good to keep it to make understand which file is this

## 13. Use of template directory

whatever file we make in template directory. will get it as template to reuse it.

## 14. Navigation file system all

- use `pwd` command to see where are you.
    
- use `ls` command to find or see subdirectories and files in directory.
    
    ls has own flags which helps us a lot like `-a` to see hidden stuff, `-l` to see info in list form and all the info about files and folders and `-h` for human readable
    
- use `cd` command to change directories.
    

## 15. Kinds of paths in linux

Absolute paths starts at the base (/) directory.

e.g. `/home/user/Desktop` this is absolute path

Relative paths starts from the current directory.

Suppose you are in `user` and wanna go in Desktop directory so rather then you type `cd /home/user/Desktop` instead you can just type `cd Desktop`. it will work fine because now it use relative path which starts from current directory.

## 16. Wildcard

A wild card is a character that can be used as a substitute for any of class of character in search, thereby greatly increasing the flexibility and efficiency of searches.

we have mainly three types of wildcard

1. `*` astric can be use as a character or as a string. This is useful when searching for documents or files but only remembering a part of its name.
2. `?` question mark is use for only one character. This is useful when you have a list of similarly named files and unsure of a few characters.
3. `[]` is used to match any occurrences of characters defined inside the brackets.

## 17. Creating files and Directories

- use `touch` command use create a empty file
    
    e.g. `touch *fileNam*`
    
- use echo to redirect content in a file and its also create that file
    
    e.g. `echo "my file content and it also create the new file" > *fileName*`
    
    if _fileNam_e file is already exist then it will rewrite it after deleting all content in _fileName_ file
    
- use `mkdir` to create a folder or directory also mkdir stands for make directory
    
    e.g. `mkdir *folderName`* and `mkdir -p /folderName/insideAFolder/againAFolder` flag -p create all these folder in a one command
    
- creating files and folder with the help of brace expansion `{}`
    
    suppose you are working on project and use need create folder according to month name for 5 year and each and every folder have 30 files withing them. so how would you do it, we can do it in just one command yeah that’s linux has incredible power. so lets use it -
    
    `mkdir {Jan,Feb,Mar,Apr,May,Jun,Jul,Agu,Sep,Oct,Nov,Dec}_{2020,2021,2022,2023,2024}`
    
    this command create 60 folder there name will be like
    
    Jan_2020
    
    Jan_2021
    
    jan_2022
    
    jan_2023
    
    jan_2024
    
    Feb_2020
    
    and so on
    
    we have also short cut for that-
    
    `mkdir {Jan,Feb,Mar,Apr,May,Jun,Jul,Agu,Sep,Oct,Nov,Dec}_{2020..2024}`
    
    this two `..` will work for any kind of sequence like {A..Z} , {a..b} , {1..100}
    
    Now going to create files withing them
    
    `touch {Jan,Feb,Mar,Apr,May,Jun,Jul,Agu,Sep,Oct,Nov,Dec}_{2020..2024}/file{1..30}`
    

## 18. Removing files and folders

`rm fileName` this will simply remove fileName file

`rm -r FolderName` this will delete that folder and all the files in this folder `-r` stands for recursively so that’s by is delete all the things recursively whatever inside the folder it too dangers command there is no to retrieve all those files and folders, for making it safe next command will help you

`rm -ri FolderName` there is `-i` stands for interactively. before deleting any file and folder it will ask you can i delete this you type `y` for allow and `n` for denial

`rm file*` there we are using * for deleting all files in current directory which matches this `file*` pattern.

`rm *[1,2]*` it will work like above one.

## 19. Coping files and folders

`cp originalFile copiedfile` copy file content into another file.

`cp fileName... <destination\\>` copy files to another place.

`cp -r folder <destination>` copy folder to another directory.

## 20. Renaming and moving files and folders

`mv oldName newName` renaming of file.

`mv fileName... <destination\\>` move file to somewhere else.

`mv -i fileName... <destination>` move file safely in another directory, where same name file is exist.

`mv folder <destination>` move folder to another folder.

`mv folder <destination>/changeNameOfFolder` moving the folder and change its name at the same time.

## 21. Change permissions of a file

- `chown` change owner and group of a file
    
    e.g. chown user:group file
    
- `chmod` change file permissions of a file
    
    - 1 execute
    - 2 written
    - 3 execute and written
    - 4 read
    - 5 execute and read
    - 6 written and read
    - 7 read, write and execute
    
    e.g. `chmod` 777 file
    
    first 7 for user, second for group and third for everyone else.
    

## 22. locate

The locate command is the quickest and simplest way to search for files and directories by their names. it is return the full path of file for that its use the databasedb.

`locate` [option] …

The locate command also accepts patterns containing globbing characters such as the wildcard character *.



## 23. operator

In linux we very powerful operators like

1. &&,    
2. &,    
3. > , >>,    
4. <, <<,    
5. |,    
6. $,    
7. ; .
    
## 24. liking

**25. viewing files :**

**26. sorting data :**

**27. searching file content :**

**28. file archive and compression :**