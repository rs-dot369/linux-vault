# Bandit Level 11 → Level 12

## Level Goal

The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Methodology

- First thing first, we need to login as bandit11.
```
└─$ ssh bandit11@bandit.labs.overthewire.org -p 2220
```
- After successful login, contents in `/home/bandit11/`
```
bandit11@bandit:~$ ls -al /home/bandit11/
total 24
drwxr-xr-x   2 root     root     4096 Jun 24 14:58 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
-rw-r-----   1 bandit12 bandit11   49 Jun 24 14:58 data.txt
```
- We need to read `data.txt`, but this encoded using `ROT13`, We will be using `cyberchef`, for this.
```
bandit11@bandit:~$ cat /home/bandit11/data.txt 
Gur cnffjbeq vf TEBbmJCB8DlA0zTewHxVQ0JPLxMvDkeA
bandit11@bandit:~$ 
```
- Just use `ROT13` operation to make the recipe on `cyberchef`.
```
The password is GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```
- We got the password for next level, bandit12.
## Key Takeaways / Concepts
- We can use CyberChef to encode or decode strings.
