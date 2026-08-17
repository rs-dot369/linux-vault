# Bandit Level 1 → Level 2

## Level Goal

The password for the next level is stored in a file called **-** located in the home directory

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

## Solution Steps/Methodology

* For this we have to read a file, dashed filename, located in home directory /home/bandit1/
* First we need to login as bandit1
```
└─$ ssh bandit1@bandit.labs.overthewire.org -p 2220

                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit1@bandit.labs.overthewire.org's password: 
...
...
...
bandit1@bandit:~$
```

* Contents of /home/bandit1

```
bandit1@bandit:~$ ls -al /home/bandit1
total 24
-rw-r-----   1 bandit2 bandit1   33 Jun 24 14:59 -
drwxr-xr-x   2 root    root    4096 Jun 24 14:59 .
drwxr-xr-x 150 root    root    4096 Jun 24 15:02 ..
-rw-r--r--   1 root    root     220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root    root    3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root    root     807 Feb 13 12:16 .profile
```

* There is a file, named -
* Contents of -

```
bandit1@bandit:~$ cat /home/bandit1/-
REDACTED
```

We got the password for bandit2.

## Key Takeaways / Concepts
* Handling dashed filenames(-)
* In Linux, commands interpret a single hyphen (`-`) as an option flag (like `cat -`) rather than a filename, which causes the command to hang or expect standard input (stdin).
* We can use, `cat ./-` (relative path) and  `cat /home/bandit1/-` (absolute path) to read files with special filenames.