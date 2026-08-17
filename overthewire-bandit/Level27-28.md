# Bandit Level 27 → Level 28

## Level Goal

There is a git repository at `ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Commands you may need to solve this level

git
## Methodology
- We have to clone a repository from git. I have cloned many repository from `https://` but never used `ssh://` to clone.
- Command: `git clone [options] protocol://git@host:port/path/to/repo`
```
└─$ git clone -v ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
Cloning into 'repo'...
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit27-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (3/3), done.
```
- Use bandit27 password that obtained before.
- We can see the contents of `repo`
```
└─$ ls -la repo
total 16
drwxrwxr-x 3 kali kali 4096 Aug 14 23:36 .
drwxrwxr-x 5 kali kali 4096 Aug 14 23:33 ..
drwxrwxr-x 7 kali kali 4096 Aug 14 23:36 .git
-rw-rw-r-- 1 kali kali   68 Aug 14 23:36 README
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ cat repo/README 
The password to the next level is: REDACTED
```
- We got the password of bandit28, next level.
## Key Takeaways / Concepts
- **Cloning via SSH with Custom Ports (`git clone`):** When interacting with a remote Git repository hosted on a non-standard SSH port, the URL scheme requires explicit port designation (e.g., `git clone ssh://user@host:port/path/to/repo`).    
- **Using Existing Level Credentials for Git Users:** CTF environments often configure auxiliary git user accounts (`bandit27-git`) to share the exact same password as the main level user (`bandit27`), allowing you to authenticate seamless source code checkouts.