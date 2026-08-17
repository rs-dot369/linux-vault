# Bandit Level 2 → Level 3

## Level Goal

The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)
## Solution Steps/Methodology
### Step1: 
* First we need to login as bandit2,
```
└─$ ssh bandit2@bandit.labs.overthewire.org -p 2220

                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit2@bandit.labs.overthewire.org's password: 
...
...
...
bandit2@bandit:~$
```
* Here, we need to read a special file, `--spaces in this filename--`
* We can use Tab key to autocomplete the filename or use escape character `\`
```
bandit2@bandit:~$ ls -al /home/bandit2
total 24
-rw-r----- 1 bandit3 bandit2 33 Jun 24 14:59 --spaces in this filename--
drwxr-xr-x 2 root root 4096 Jun 24 14:59 .
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
-rw-r--r-- 1 root root 220 Feb 13 12:16 .bash_logout
-rw-r--r-- 1 root root 3851 Jun 24 14:50 .bashrc
-rw-r--r-- 1 root root 807 Feb 13 12:16 .profile
```
### Step 2:
* Reading the contents of the file
```
bandit2@bandit:~$ cat /home/bandit2/--spaces\ in\ this\ filename--
REDACTED
```
* We can move to the next level 3 to 4, i.e, level 3 with the password of bandit3.
## Key Takeaways / Concepts
* Filename may contain spaces or whitespaces, to read these type of files we can use escape sequence character '\' or simply use Tab to auto complete.