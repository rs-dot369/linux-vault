# Bandit Level 3 → Level 4

## Level Goal

The password for the next level is stored in a hidden file in the **inhere** directory.

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

## Methodology

* This time we have to read files in `inhere` directory.
* After login as bandit3,
```
bandit3@bandit:~$ find / -type d -name "inhere" 2>/dev/null
/home/bandit5/inhere
/home/bandit3/inhere
/home/bandit4/inhere
```
* With permissions,
```
bandit3@bandit:~$ ls -al /home/bandit5/inhere/ /home/bandit3/inhere/ /home/bandit4/inhere/
/home/bandit3/inhere/:
total 12
drwxr-xr-x 2 root root 4096 Jun 24 14:59 .
drwxr-xr-x 3 root root 4096 Jun 24 14:59 ..
-rw-r----- 1 bandit4 bandit3 33 Jun 24 14:59 ...Hiding-From-You
/home/bandit4/inhere/:
total 48
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file00
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file01
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file02
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file03
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file04
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file05
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file06
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file07
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file08
-rw-r----- 1 bandit5 bandit4 33 Jun 24 14:59 -file09
drwxr-xr-x 2 root root 4096 Jun 24 14:59 .
drwxr-xr-x 3 root root 4096 Jun 24 14:59 ..
ls: cannot open directory '/home/bandit5/inhere/': Permission denied
```
* We have to read `...Hiding-From-You` 
```
bandit3@bandit:~$ cat /home/bandit3/inhere/...Hiding-From-You 
REDACTED
bandit3@bandit:~$ 
```
* We got the password for the next level, bandit4.
## Key Takeaways / Concepts
* File name may contain .`...file-name` to hide them.
* We can use `find / -type d -name "inhere"` to find directories.