# Bandit Level 0 → Level 1

## Level Goal

The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

# Solutions Steps/Methodology

The password for the next level is stored in a file called **readme** located in the home directory

- we have to read readme file in home directory but we don't know in which directory because i'm seeing many directories in /home directory

we have to find it,

```
bandit0@bandit:~$ find /home -name "readme" 2>/dev/null
/home/bandit0/readme
/home/bandit18/readme
```

okay we have found two readme, let's try to read them, i think we to check persmission of these files to check if we can read or not?
```
bandit0@bandit:~$ ls -l /home/bandit0/readme
-rw-r----- 1 bandit1 bandit0 438 Jun 24 14:58 /home/bandit0/readme
```

yes we can read it, and here it is

```
bandit0@bandit:~$ cat /home/bandit0/readme

Congratulations on your first steps into the bandit game!!

Please make sure you have read the rules at [https://overthewire.org/rules/](https://overthewire.org/rules/)

If you are following a course, workshop, walkthrough or other educational activity,

please inform the instructor about the rules as well and encourage them to

contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: 6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
```

Level 1 is complete.

### Additional Work

On the welcome screen it's mentioned that all pass are store into /etc/bandit_pass.
```
bandit1@bandit:~$ ls -l /etc/bandit_pass
total 136
-r-------- 1 bandit0 bandit0 8 Jun 24 14:58 bandit0
-r-------- 1 bandit1 bandit1 33 Jun 24 14:58 bandit1
-r-------- 1 bandit10 bandit10 33 Jun 24 14:58 bandit10
-r-------- 1 bandit11 bandit11 33 Jun 24 14:58 bandit11
-r-------- 1 bandit12 bandit12 33 Jun 24 14:58 bandit12
-r-------- 1 bandit13 bandit13 33 Jun 24 14:58 bandit13
-r-------- 1 bandit14 bandit14 33 Jun 24 14:58 bandit14
-r-------- 1 bandit15 bandit15 33 Jun 24 14:58 bandit15
-r-------- 1 bandit16 bandit16 33 Jun 24 14:58 bandit16
-r-------- 1 bandit17 bandit17 33 Jun 24 14:58 bandit17
-r-------- 1 bandit18 bandit18 33 Jun 24 14:58 bandit18
-r-------- 1 bandit19 bandit19 33 Jun 24 14:58 bandit19
-r-------- 1 bandit2 bandit2 33 Jun 24 14:58 bandit2
-r-------- 1 bandit20 bandit20 33 Jun 24 14:58 bandit20
-r-------- 1 bandit21 bandit21 33 Jun 24 14:58 bandit21
-r-------- 1 bandit22 bandit22 33 Jun 24 14:58 bandit22
-r-------- 1 bandit23 bandit23 33 Jun 24 14:58 bandit23
-r-------- 1 bandit24 bandit24 33 Jun 24 14:58 bandit24
-r-------- 1 bandit25 bandit25 33 Jun 24 14:58 bandit25
-r-------- 1 bandit26 bandit26 33 Jun 24 14:58 bandit26
-r-------- 1 bandit27 bandit27 33 Jun 24 14:58 bandit27
-r-------- 1 bandit28 bandit28 33 Jun 24 14:58 bandit28
-r-------- 1 bandit29 bandit29 33 Jun 24 14:58 bandit29
-r-------- 1 bandit3 bandit3 33 Jun 24 14:58 bandit3
-r-------- 1 bandit30 bandit30 33 Jun 24 14:58 bandit30
-r-------- 1 bandit31 bandit31 33 Jun 24 14:58 bandit31
-r-------- 1 bandit32 bandit32 33 Jun 24 14:58 bandit32
-r-------- 1 bandit33 bandit33 33 Jun 24 14:58 bandit33
-r-------- 1 bandit4 bandit4 33 Jun 24 14:58 bandit4
-r-------- 1 bandit5 bandit5 33 Jun 24 14:58 bandit5
-r-------- 1 bandit6 bandit6 33 Jun 24 14:58 bandit6
-r-------- 1 bandit7 bandit7 33 Jun 24 14:58 bandit7
-r-------- 1 bandit8 bandit8 33 Jun 24 14:58 bandit8
-r-------- 1 bandit9 bandit9 33 Jun 24 14:58 bandit9
```
But we don't have permissions to read, So we have to move step by step.

## Key Takeaways / Concepts
* **Finding Files:** Using `find /home -name "filename" 2>/dev/null` lets us search across directories while hiding "Permission denied" errors to keep our terminal clean as we're directing errors to void.
* **The Master Password Directory:** Passwords for all levels are stored centrally in `/etc/bandit_pass/`, but are restricted so that we can only read them once you've successfully escalated to that specific user level.