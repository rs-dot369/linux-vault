# Bandit Level 22 → Level 23

## Level Goal

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

## Commands you may need to solve this level

cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution Steps/Methodology
### Step 1:
For this level, we've also deal with cron jobs, let's login as bandit22 first.
```
└─$ ssh bandit22@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit22@bandit.labs.overthewire.org's password: 

```

Starting with exploring /home/bandit22
```
bandit22@bandit:~$ ls -al /home/bandit22
total 20
drwxr-xr-x   2 root root 4096 Jun 24 14:58 .
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
-rw-r--r--   1 root root  220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root root  807 Feb 13 12:16 .profile
```
Nothing special here.
Moving to main target, cron jobs.
```
bandit22@bandit:~$ ls -al /etc/cron.d/
total 56
drwxr-xr-x   2 root root  4096 Jul  3 16:19 .
drwxr-xr-x 124 root root 12288 Jun 25 12:37 ..
-rw-r--r--   1 root root   102 Nov  5  2025 .placeholder
-r--r-----   1 root root    47 Jun 24 14:59 behemoth4_cleanup
-rw-r--r--   1 root root   127 Jul  3 16:19 clean_tmp
-rw-r--r--   1 root root   120 Jun 24 14:58 cronjob_bandit22
-rw-r--r--   1 root root   122 Jun 24 14:58 cronjob_bandit23
-rw-r--r--   1 root root   120 Jun 24 14:59 cronjob_bandit24
-rw-r--r--   1 root root   188 Feb 13 12:17 e2scrub_all
-r--r-----   1 root root    48 Jun 24 15:00 leviathan5_cleanup
-rw-------   1 root root   138 Jun 24 15:01 manpage3_resetpw_job
-rwx------   1 root root    52 Jun 24 15:02 otw-tmp-dir
```

here, cronjob_bandit23, seems useful.
```
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ ls -al /usr/bin/cronjob_bandit23.sh 
-rwxr-x--- 1 bandit23 bandit22 211 Jun 24 14:58 /usr/bin/cronjob_bandit23.sh
```

We have read and execute permission on /usr/bin/cronjob_bandit23.sh

```
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh 
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

At first sight, it seems easy, execute this script, got the answer, NO.
```
#!/bin/bash

myname=$(whoami)  #get the username of current user i.e, bandit22
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)  # here we are converting the text into md5sum hash, used as filename for storing the password

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget  #/etc/bandit_pass/bandit22 > /tmp/filename , filename, the hash that was generated previously
```

when we, bandit22, run this script, it will direct us to a file that stores our, bandit22, password.
### Step 2:
Verifying the observations.
```
bandit22@bandit:~$ /usr/bin/cronjob_bandit23.sh 
Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f
bandit22@bandit:~$ cat /tmp/8169b67bd894ddbb4412f91573b38db3
RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz
bandit22@bandit:~$ 
bandit22@bandit:~$ echo I am user bandit22 | md5sum | cut -d ' ' -f 1
8169b67bd894ddbb4412f91573b38db3
```
Verified.

```
bandit22@bandit:~$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

This is the file in /tmp, 8ca319486bfbbc3663ea0fbe81326349, that stores the password for bandit23.

```
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw
```

Using this password to login as bandit23, just to verify if this password is correct or not.
```
└─$ ssh bandit23@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit23@bandit.labs.overthewire.org's password: 
...
...
...
  Enjoy your stay!

bandit23@bandit:~$

```
Verified.
## Key Takeaways / Concepts
* **Dynamic Script Analysis:** Reading scripts written by others (like `/usr/bin/cronjob_bandit23.sh`) lets you trace how variables are computed—such as using `whoami` and `md5sum` to dynamically generate randomized or hashed filenames.
* **Privilege Context in Cron:** When a cron job runs, it executes under the privileges of the specified user (`bandit23` in this case). If the script relies on `whoami` internally, executing it manually as your current user (`bandit22`) generates *your* target path, whereas letting the cron job run automatically (or calculating the target for the *next* user) reveals the upcoming flag file.
