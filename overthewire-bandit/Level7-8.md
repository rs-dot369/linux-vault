# Bandit Level 7 → Level 8

## Level Goal

The password for the next level is stored in the file **data.txt** next to the word **millionth**

## Commands you may need to solve this level

[man](https://manpages.ubuntu.com/manpages/noble/man1/man.1.html), grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Methodology
* I think this level and previous level is easy peasy.
* First thing first, we have to login as bandit7 with password obtained in previous level.
```
└─$ ssh bandit7@bandit.labs.overthewire.org -p 2220
```

```
bandit7@bandit:~$ ls -al /home/bandit7
total 4108
drwxr-xr-x   2 root    root       4096 Jun 24 14:59 .
drwxr-xr-x 150 root    root       4096 Jun 24 15:02 ..
-rw-r--r--   1 root    root        220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root    root       3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root    root        807 Feb 13 12:16 .profile
-rw-r-----   1 bandit8 bandit7 4184396 Jun 24 14:59 data.txt
```
* After login, we can proceed, this time we need to read `data.txt`, we'll be using `grep` to filter the line.
```
bandit7@bandit:~$ cat data.txt | grep "millionth"
millionth       REDACTED
bandit7@bandit:~$ 
```
* We got the password for next level, bandt8.
## Key Takeaways / Concepts
* We can use `grep` to filter the line containing specific pattern or text.