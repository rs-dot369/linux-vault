# Bandit Level 8 → Level 9

## Level Goal

The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Methodology
- This time we also need to read `data.txt`.
- After successful login as bandit8.
```
└─$ ssh bandit8@bandit.labs.overthewire.org -p 2220 
```

```
bandit8@bandit:~$ ls -al /home/bandit8/
total 56
drwxr-xr-x   2 root    root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root    root     4096 Jun 24 15:02 ..
-rw-r--r--   1 root    root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root    root     3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root    root      807 Feb 13 12:16 .profile
-rw-r-----   1 bandit9 bandit8 33033 Jun 24 14:59 data.txt
```
- `data.txt` is too long to read using `cat`.
-  only line of text that occurs only once, means it's unique line, we can use `uniq` but `uniq` won't work unless data.txt is sorted, we have to use `sort`.
```
bandit8@bandit:~$ cat /home/bandit8/data.txt | sort | uniq -u
EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
bandit8@bandit:~$ 
```
- We got the password for next level, bandit9.
## Key Takeaways / Concepts

- We can sort the content of the file or files using `sort` and find unique using `uniq -u`
- We can use piping (`|`) to use output of one command as input of another command