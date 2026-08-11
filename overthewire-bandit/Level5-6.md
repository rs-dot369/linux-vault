# Bandit Level 5 → Level 6

## Level Goal

The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

## Methodology
* First thing first, login as bandit5
```
└─$ ssh bandit5@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit5@bandit.labs.overthewire.org's password: 

```
* For this level, we have to read a file stored in `inhere` directory with following properties, 
	*  human-readable
	- 1033 bytes in size
	- not executable
* We have 20 files in `inhere` directory, 
```
bandit5@bandit:~$ ls -al /home/bandit5/inhere/
total 88
drwxr-x--- 22 root bandit5 4096 Jun 24 14:59 .
drwxr-xr-x  3 root root    4096 Jun 24 14:59 ..
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere00
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere01
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere02
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere03
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere04
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere05
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere06
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere07
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere08
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere09
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere10
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere11
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere12
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere13
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere14
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere15
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere16
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere17
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere18
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere19
```
* We can use `find` with `-type f`, `-size`, `-not -executable` 
```
bandit5@bandit:~$ find /home/bandit5/inhere -type f -size 1033c -not -executable
/home/bandit5/inhere/maybehere07/.file2
```
* It is the file we need to read as we get only one file it's obivious, but we don't know if it's readable or not
```
bandit5@bandit:~$ find /home/bandit5/inhere -type f -size 1033c -not -executable -exec file {} + | grep ASCII
/home/bandit5/inhere/maybehere07/.file2: ASCII text, with very long lines (1000)
bandit5@bandit:~$ cat /home/bandit5/inhere/maybehere07/.file2
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
                                                                               bandit5@bandit:~$ 
bandit5@bandit:~$ 
```
* We got the password for next level, bandit6.
## Key Takeaways / Concepts
* We can leverage specific options/flags of find, to find the file with specific properties, `-type f ` `-size 1033c` `-not -executable`
* We can also use `-exec <command> {} +`  to with `find` to perform other operations on output of `find`
* `-exec command {} \;` → “for `each match`, run `command` separately”
- `-exec command {} +` → “run `command` with as many `{}` matches as possible per call”
* We can filter using `grep` 