# Bandit Level 19 → Level 20

## Level Goal

To gain access to the next level, you should use the setuid binary in the home directory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

## Helpful Reading Material

- [setuid on Wikipedia](https://en.wikipedia.org/wiki/Setuid)
## Methodology
- `setuid`, is file permission bit that allows a user to execute a program with the permissions of the file's owner rather than the user running it. It is commonly used to grant temporary, privileged access.
- We can also verify this by looking the permissions (`s`)
```
bandit19@bandit:~$ ls -al
total 36
drwxr-xr-x   2 root     root      4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root      4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root       220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root      3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root     root       807 Feb 13 12:16 .profile
-rwsr-x---   1 bandit20 bandit19 14880 Jun 24 14:59 bandit20-do
```
- Whenever we run this binary, it will be executed as if `bandit20` is executing it.
```
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do whoami
bandit19@bandit:~$ ./bandit20-do whoami
bandit20
```
- We can directly read the contents of `/etc/bandit_pass/bandit20` , because when we will be reading the file that was supposed to read by only bandit20, we will read it as bandit20.
```
bandit19@bandit:~$ ls -l /etc/bandit_pass/bandit20
-r-------- 1 bandit20 bandit20 33 Jun 24 14:58 /etc/bandit_pass/bandit20
```

```
bandit19@bandit:~$ ./bandit20-do  cat /etc/bandit_pass/bandit20
REDACTED
```
- We got the password of bandit20, next level.
## Key Takeaways / Concepts
* **SetUID Execution (`s` permission bit):** Files with the SetUID bit set (displayed as an `s` instead of an `x` in file permissions, e.g., `-rwsr-x---`) run with the privileges of the file's **owner** rather than the user executing them. 
* **Privilege Elevation via Wrapper Binaries:** When a binary is owned by a higher-privileged user (like `bandit20`) and has the SetUID bit enabled, passing commands to it (like `./bandit20-do cat ...`) allows you to interact with protected resources or restricted system files normally inaccessible to your current user account.