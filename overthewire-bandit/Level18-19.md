# Bandit Level 18 → Level 19

## Level Goal

The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

## Commands you may need to solve this level

ssh, ls, cat
## Methodology
- When we tried to login as usual, it automatically logged out us.
```
└─$ ssh bandit18@bandit.labs.overthewire.org -p 2220                  
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit18@bandit.labs.overthewire.org's password: 
...
...
...
  Enjoy your stay!

Byebye !
Connection to bandit.labs.overthewire.org closed.
```
- Because, someone has modified **.bashrc** to log you out when you log in with SSH.
- Who need a interactive shell? We can read `readme` using pseudo shell.
```
└─$ ssh bandit18@bandit.labs.overthewire.org -p 2220 -t '/bin/sh'               

                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit18@bandit.labs.overthewire.org's password: 
$ ls -al
total 24
drwxr-xr-x   2 root     root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r-----   1 bandit19 bandit18 3874 Jun 24 14:59 .bashrc
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
-rw-r-----   1 bandit19 bandit18   33 Jun 24 14:59 readme
$ cat readme
REDACTED
$ whoami
bandit18
```
- We got the password for bandit19, next level.
## Additional Work
- We can directly read the require file without spawning a real session.
```
└─$ ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'               
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit18@bandit.labs.overthewire.org's password: 
REDACTED
```

- When tried to get an interactive shell within pseudo shell,
```
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
Byebye !
$ echo "Okay, the reason, we are getting out because we are logging using as ssh and ssh gives an iteractive session, that's why"
Okay, the reason, we are getting out because we are logging using as ssh and ssh gives an iteractive session, that's why
$ exit
Connection to bandit.labs.overthewire.org closed.
```
## Key Takeaways / Concepts
* **Bypassing Restrictive Shells (`-t`):** When `.bashrc` or profile scripts are modified to immediately terminate interactive sessions upon login, you can bypass them by explicitly specifying a command or a clean shell interpreter to execute via SSH (e.g., `ssh user@host -t '/bin/sh'`). 
* **Non-Interactive Remote Execution:** You do not always need a full interactive shell to retrieve information. Passing a command directly after the SSH destination string (e.g., `ssh user@host 'cat readme'`) executes the command non-interactively, prints the output locally, and exits immediately before any logout triggers can run.