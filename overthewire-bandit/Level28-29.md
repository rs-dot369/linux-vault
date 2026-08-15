# Bandit Level 28 → Level 29

## Level Goal

There is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Commands you may need to solve this level

git
## Methodology
- First we have to clone the required repo using ssh on our machine, use the credentials of bandit28.
```
git clone -v ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
```
- Contents of `repo`
```
└─$ ls -al 
total 16
drwxrwxr-x 3 kali kali 4096 Aug 15 02:01 .
drwxrwxr-x 6 kali kali 4096 Aug 14 23:57 ..
drwxrwxr-x 7 kali kali 4096 Aug 14 23:57 .git
-rw-rw-r-- 1 kali kali  111 Aug 14 23:57 README.md
```
- We can read the `README.md`
```
└─$ cat README.md     
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
    
```
- But password is not here. While writing `README.md` , maybe it's written like this or maybe edited after that.
- We have to look into log of commits.
```
└─$ git log                 
commit 83d77407b76c9f86ac4e691a47618641c9d527ba (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jun 24 14:59:06 2026 +0000

    fix info leak

commit 13bbc4d2414ffe0439b8ee4f5e5c2949780cf4b3
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jun 24 14:59:06 2026 +0000

    add missing data

commit f3334fbccbf9446a6af88a3c71021c2f57163322
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jun 24 14:59:06 2026 +0000

    initial commit of README.md
                                                    
```
- We can see three commits, the theory is, in first commit they created README.md, in second commit, they added the password and in third commit, they replaced the password with xxxxxxxxxxxxxx.
- We can directly see the `add missing data` commit, to check what they added, if this commit won't give password we have to check all the commits one by one.
- To switch between commits we can hash of that commit with `git checkout <hash>` or `git switch --detach <hash>`, use full hash or first 6 six character will also work.
```
└─$ git switch --detach 13bbc4d2414ff                           
Previous HEAD position was 83d7740 fix info leak
HEAD is now at 13bbc4d add missing data
                                                                                                                                                                      
└─$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: Em7eGtqaMySwNFjCpwzzHhLhospOcdt0


```
- We got the password of bandit29, next level.
## Additional Work
- Exploring other commits in sequence.
- commit: `initial commit of README.md`
```
└─$ git switch --detach f3334fbccbf9446a6af88a3c71021c2f57163322
Previous HEAD position was 13bbc4d add missing data
HEAD is now at f3334fb initial commit of README.md
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: <TBD>

```
- commit: `add missing data`
```
└─$ git switch --detach 13bbc4d2414ffe0439b8ee4f5e5c2949780cf4b3
Previous HEAD position was f3334fb initial commit of README.md
HEAD is now at 13bbc4d add missing data
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: Em7eGtqaMySwNFjCpwzzHhLhospOcdt0

```
- commit: `fix info leak`
```
└─$ git checkout 83d77407                                       
Previous HEAD position was 13bbc4d add missing data
HEAD is now at 83d7740 fix info leak
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
## Key Takeaways / Concepts
- **Inspecting Commit Logs (`git log`):** When sensitive information or passwords are missing from current files in a repository, analyzing the commit history using `git log` helps identify past states where secrets might have been accidentally committed before being scrubbed or masked.
- **Navigating Historical Commits (`git switch --detach` or `git checkout`):** You can travel backward through a repository's timeline by checking out specific commit hashes (or their unique prefixes). This allows you to inspect files exactly as they existed at any point in the project's history.
