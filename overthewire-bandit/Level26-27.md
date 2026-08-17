# Bandit Level 26 → Level 27

## Level Goal

Good job getting a shell! Now hurry and grab the password for bandit27!

## Commands you may need to solve this level

ls
## Methodology
- First thing first, we have to login as bandit26, with the obtained password in previous level.
```
└─$ ssh bandit26@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit26@bandit.labs.overthewire.org's password: 

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
- But we are getting out as previously. It was expected.
- We have to use previous method. Make the terminal window size small so that all text don't show up in single page. Press `v` to open `vi`,  in command mode,`:set shell=/bin/bash`, `:shell`. Maximize the terminal window.
```
 └─$ ssh bandit26@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit26@bandit.labs.overthewire.org's password: 
...
  Enjoy your stay!

  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
:shell
bandit26@bandit:~$ whoami
bandit26 
```
- We got the shell as bandit26, we have to be in this shell.
```
bandit26@bandit:~$ ls -al                                                        
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
- We have `setuid` executable, and owner is bandit27.
```
bandit26@bandit:~$ ./bandit27-do                                                 
Run a command as another user.                                                   
  Example: ./bandit27-do id                                                      
bandit26@bandit:~$ ./bandit27-do id                                              
uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit2
bandit26@bandit:~$ ./bandit27-do whoami
bandit27                                       
```
- We can read the `/etc/bandit_pass/bandit27` for the password of bandit27.
```                         
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
REDACTED                                                 
bandit26@bandit:~$
bandit26@bandit:~$ ./bandit27-do ls -al /home/bandit27
total 20                                                                         
drwxr-xr-x   2 root root 4096 Jun 24 14:58 .                                     
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..                                    
-rw-r--r--   1 root root  220 Feb 13 12:16 .bash_logout                          
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc                               
-rw-r--r--   1 root root  807 Feb 13 12:16 .profile  
```
- We got the password of bandit27, next level.
## Key Takeaways / Concepts
- Successfully executing custom shell breakouts from restricted environments (using text viewers like `more` and text editors like `vi` to modify shell variables and spawn shells) to leverage privileged SetUID binaries.
- Using custom tools like `bandit27-do` to execute commands and retrieve sensitive credential files as the target user.