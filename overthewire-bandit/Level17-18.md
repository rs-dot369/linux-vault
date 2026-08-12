# Bandit Level 17 → Level 18

## Level Goal

There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

## Commands you may need to solve this level

cat, grep, ls, diff
## Methodology
- Password for next level, bandit18, is in `passwords.new`, and only one line is different from `passwords.old`, and that is the password we need.
- For that we can use `diff`, to find difference between to files.
```
bandit17@bandit:~$ ls -l
total 8
-rw-r----- 1 bandit18 bandit17 3300 Jun 24 14:59 passwords.new
-rw-r----- 1 bandit18 bandit17 3300 Jun 24 14:59 passwords.old
bandit17@bandit:~$ diff passwords.new passwords.old 
42c42
< OQxXZjELndr90zuhOTDYBEomI0SZITXI				
---
> icUh23IUytZLIYhcCaXL18agiSIqymBc

```
- We got the password for bandit18, next level.
## Additional Work
- As we logged in as bandit17, we can read `/etc/bandit_pass/bandit17` for bandit17's password.
```
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
pWXMAZoxGC8JmDMfmT5MGEsobMM3vnj2
```
- We have now bandit17's password.
## Key Takeaways / Concepts
* **File Comparison (`diff`):** The `diff` utility compares two files line by line, making it easy to spot changes, additions, or deletions between old and new versions of data sets. 
* **Interpreting Diff Output:** 
	* `<`: Denotes lines unique to or changed in the first file. 
	* `>`: Denotes lines unique to or changed in the second file.