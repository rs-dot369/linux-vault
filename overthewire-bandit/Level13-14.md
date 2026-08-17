# Bandit Level 13 → Level 14

## Level Goal

The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.  
If you need help with this level: a hint file can be found in the home directory.  
Make sure to read the error messages as they are informative.

## Commands you may need to solve this level

ssh, scp, umask, chmod, cat, nc, install

## Methodology
- For this level, we have to use private SSH key to login in next level, we can do that, first we have to find it, make a copy of the key on our machine, set permissions.
- First thing first, we have to login as bandit13.
```
└─$ ssh bandit13@bandit.labs.overthewire.org -p 2220
```
- Contents of `/home/bandit13/`
```
bandit13@bandit:~$ ls -al /home/bandit13/
total 28
drwxr-xr-x   2 root     root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
-rw-r-----   1 bandit14 bandit13  467 Jun 24 14:59 HINT
-rw-r-----   1 bandit14 bandit13 2602 Jun 24 14:59 sshkey.private
```
- Make a copy of sshkey.private, by copying pasting or `scp` to transfer the file on our machine.
- On our machine,
```
──(kali㉿kali)-[~/Desktop/overthewire/bandit]
└─$ nano bandit14-privatekey    
┌──(kali㉿kali)-[~/Desktop/overthewire/bandit]
└─$ chmod 600 bandit14-privatekey 
┌──(kali㉿kali)-[~/Desktop/overthewire/bandit]
└─$ ssh -i bandit14-privatekey bandit14@bandit.labs.overthewire.org -p 2220
```
- We have a session as bandit14 without any password, we are already on next level.
## Additional Work
- We have already session as bandit14, we can read the contents of */etc/bandit_pass/bandit14*
```
bandit14@bandit:~$ ls -al /etc/bandit_pass/bandit14 
-r-------- 1 bandit14 bandit14 33 Jun 24 14:58 /etc/bandit_pass/bandit14
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
REDACTED
```
## Key Takeaways / Concepts
* **SSH Key Authentication (`ssh -i`):** We can log into an SSH server using a private key instead of a password by specifying the identity file with the `-i` flag (e.g., `ssh -i keyfile user@host`).
* **Strict Key Permissions (`chmod 600`):** SSH enforces strict security on private keys. The key file must have restricted permissions (read/write for the owner *only*), which we set using `chmod 600`. If the file is readable by other users, the SSH client will refuse to use it and throw a "bad permissions" error.
* **File Transfer Alternatives:** To get files from a remote server to a local machine, we can either use file transfer tools like `scp`, or simply print the file on the terminal (`cat`) and paste its contents into a local text editor (`nano`).