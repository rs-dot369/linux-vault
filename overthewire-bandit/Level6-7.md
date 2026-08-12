# Bandit Level 6 → Level 7

## Level Goal

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html) , [grep](https://manpages.ubuntu.com/manpages/noble/man1/grep.1.html)

## Methodology
* First thing first, we have to login as bandit6.
* After login, We have to read a find a file and read it, with some given properties.
* For Owner, we can use `-user` , for group `-group`, for size `-size`, 
```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null -exec ls -al {} +
-rw-r----- 1 bandit7 bandit6 33 Jun 24 14:59 /var/lib/dpkg/info/bandit7.password
```
* `2>/dev/null` redirects error messages to void/blackhole.
* Content of `/var/lib/dpkg/info/bandit7.password`
```
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
```
* We got the password for next level, bandit7.
## Key Takeaways / Concepts
* Used `find`, flags `-user`,` -group` to find files owned by user and group.
* `2>/dev/null` used for clean terminal means no more error messages.