# Bandit Level 9 → Level 10

## Level Goal

The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Methodology
- Again `data.txt`, but here data.txt contains some non human readable text, but we only need human readable, we can use `strings`
```
bandit9@bandit:~$ strings /home/bandit9/data.txt 
j"+V[
je}<
plM&]
*-D@6@
Y}fti
b>L*
z]U}yb
=KGEn
xYnz
zch]U
R*`\
cL0========== the
...
...
...
```
- But the file is long enough, we need to filter, using `grep`
```
bandit9@bandit:~$ strings /home/bandit9/data.txt | grep "=="
cL0========== the
========== password
>========== is
R========== REDACTED
bandit9@bandit:~$ 
```
- We got the password for next level, bandit9.
## Key Takeaways / Concepts
- We can use `strings`, to extract human-readable text from files that are otherwise “non-human-readable” (like binaries, executables, images with embedded metadata, firmware, etc.).