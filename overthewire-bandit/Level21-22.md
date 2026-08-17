# Bandit Level 21 → Level 22

## Level Goal

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

## Commands you may need to solve this level

cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution Steps/Methodology
### Step 1:
It was told that we need to look in /etc/cron.d, but before that we need to understand what is cron jobs, so cron jobs are simply scheduled jobs/task that run on specific given time.

Before going to cron jobs, let's look in /home/bandit21 for other hidden files. And yes we have to login as bandit21 using ssh before doing anything.

```
└─$ ssh bandit21@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit21@bandit.labs.overthewire.org's password: 

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit21@bandit:~$
```

After successful login, we can proceed now

```
bandit21@bandit:~$ ls -al /home/bandit21
total 24
drwxr-xr-x   2 root     root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-r--------   1 bandit21 bandit21   33 Jun 24 14:59 .prevpass
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
bandit21@bandit:~$ cat .prevpass 
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```

Nothing here just password for bandit20, previous level.

### Step 2:
Going after cron jobs,

```
bandit21@bandit:~$ ls -al /etc/crontab 
-rw-r--r-- 1 root root 1136 Nov  5  2025 /etc/crontab
bandit21@bandit:~$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.daily; }
47 6    * * 7   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.weekly; }
52 6    1 * *   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.monthly; }
#

```

Nothing here, we can now go after /etc/cron.d/

```
bandit21@bandit:~$ cat /etc/cron
cron.d/       cron.hourly/  crontab       cron.yearly/  
cron.daily/   cron.monthly/ cron.weekly/  
bandit21@bandit:~$ cat /etc/cron.d
cat: /etc/cron.d: Is a directory
bandit21@bandit:~$ cd /etc/cron.d
cron.d/     cron.daily/ 
bandit21@bandit:~$ cd /etc/cron.d/
bandit21@bandit:/etc/cron.d$ ls -al
total 56
drwxr-xr-x   2 root root  4096 Jul  3 16:19 .
drwxr-xr-x 124 root root 12288 Jun 25 12:37 ..
-rw-r--r--   1 root root   102 Nov  5  2025 .placeholder
-r--r-----   1 root root    47 Jun 24 14:59 behemoth4_cleanup
-rw-r--r--   1 root root   127 Jul  3 16:19 clean_tmp
-rw-r--r--   1 root root   120 Jun 24 14:59 cronjob_bandit22
-rw-r--r--   1 root root   122 Jun 24 14:59 cronjob_bandit23
-rw-r--r--   1 root root   120 Jun 24 14:59 cronjob_bandit24
-rw-r--r--   1 root root   188 Feb 13 12:17 e2scrub_all
-r--r-----   1 root root    48 Jun 24 15:01 leviathan5_cleanup
-rw-------   1 root root   138 Jun 24 15:01 manpage3_resetpw_job
-rwx------   1 root root    52 Jun 24 15:03 otw-tmp-dir
```

cronjob_bandit22 seems useful, let's look into this,

```
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

/usr/bin/cronjob_bandit22.sh is the one that we were looking for, this shell script is running every minute.

```
bandit21@bandit:/etc/cron.d$ ls -l /usr/bin/cronjob_bandit22.sh 
-rwxr-x--- 1 bandit22 bandit21 130 Jun 24 14:59 /usr/bin/cronjob_bandit22.sh
bandit21@bandit:/etc/cron.d$ /usr/bin/cronjob_bandit22.sh 
/usr/bin/cronjob_bandit22.sh: line 3: /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv: Permission denied
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

After reading the contents of /etc/bin/cronjob_bandit22.sh, it's known that, here we are reading /etc/bandit_pass/bandit22 and writing to /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv.

```
bandit21@bandit:/etc/cron.d$ ls -l /etc/bandit_pass/bandit22
-r-------- 1 bandit22 bandit22 33 Jun 24 14:58 /etc/bandit_pass/bandit22
bandit21@bandit:/etc/cron.d$ ls -l /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
-rw-r--r-- 1 bandit22 bandit22 33 Aug  9 06:22 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
REDACTED
bandit21@bandit:/etc/cron.d$ echo "i think we got the password for next level"
i think we got the password for next level
bandit21@bandit:/etc/cron.d$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```

This level is complete, moving to the next level.

## Key Takeaways / Concepts 
* Cron jobs run commands automatically at set intervals. 
* Files in `/etc/cron.d/` define who runs the job and what script gets executed. 
* Always inspect cron scripts to see if they expose restricted files (like passwords in `/etc/bandit_pass/`).
