# Bandit Level 31 → Level 32

## Level Goal

There is a git repository at `ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo` via the port `2220`. The password for the user `bandit31-git` is the same as for the user `bandit31`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Commands you may need to solve this level

git

## Methodology
- This time we have to work with Git.
- Let's clone the required repo.
```
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo repo31-32
Cloning into 'repo31-32'...
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit31-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (4/4), done.
```
- contents of the repo31-32
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ ls -al
total 20
drwxrwxr-x 3 kali kali 4096 Aug 16 08:00 .
drwxrwxr-x 9 kali kali 4096 Aug 16 07:59 ..
drwxrwxr-x 7 kali kali 4096 Aug 16 08:00 .git
-rw-rw-r-- 1 kali kali    6 Aug 16 08:00 .gitignore
-rw-rw-r-- 1 kali kali  147 Aug 16 08:00 README.md
```
- this time we have `.gitignore`, so `.gitignore` tells git what not to add to the repo in simple terms.
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ cat README.md       
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```
- This time we have to add/push a file to the remote repository, but which file? Here is also some details, do we need to look for `key.txt`?
- Okay, Let's have a look into `.gitignore`, what it is excluding.
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ cat .gitignore      
*.txt
```
- It's excluding `.txt`, and we need to push a file,  but which file.
- Okayyy, we need to push `key.txt` with content "`May I come in?`" to `master` branch, if i'm not wrong.
```                
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ echo "May I come in?">key.txt         
                                                                    
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ ls -al
total 24
drwxrwxr-x 3 kali kali 4096 Aug 16 08:05 .
drwxrwxr-x 9 kali kali 4096 Aug 16 07:59 ..
drwxrwxr-x 7 kali kali 4096 Aug 16 08:00 .git
-rw-rw-r-- 1 kali kali    6 Aug 16 08:00 .gitignore
-rw-rw-r-- 1 kali kali   15 Aug 16 08:05 key.txt
-rw-rw-r-- 1 kali kali  147 Aug 16 08:00 README.md
```
- We have to remove *.txt* from `.gitignore`, so that when we push git doesn't exclude `key.txt`
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ nano .gitignore         
                                                                    
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ cat .gitignore      
              
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ git add key.txt 
```
- But before committing anything, we have to make configurations to avoid errors. We have to tell Git, who we are or who is committing.
```
──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ git config --global user.email "ex@gmail.com"
                                                                    
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ git config --global user.name "ex"           
```
- Not necessary to give real email and name.
- Now we can commit,
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ git add .                         
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ git commit -m "add key.txt"       
[master 966d70f] add key.txt
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 key.txt
                                                                                 
┌──(kali㉿kali)-[~/Desktop/overthewire/repo31-32]
└─$ git push origin master     
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit31-git@bandit.labs.overthewire.org's password: 
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 322 bytes | 322.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://bandit.labs.overthewire.org:2220/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit.labs.overthewire.org:2220/home/bandit31-git/repo'                                                                
```
- Although, our push is rejected but we have successfully got the password for bandit32, next level.
## Key Takeaways / Concepts
- **Bypassing File Exclusions (`.gitignore`):** When a repository's `.gitignore` file explicitly blocks certain file types (like `*.txt`), you must modify or clear the exclusion rule before Git will stage and track the required submission files.
- **Pushing Local Changes to Remote (`git push`):** Pushing local commits (`git commit -m "message"`) to a remote repository (`git push origin master`) uploads your work back to the server. In CTF environments, server-side pre-receive hooks often intercept these pushes, validate the contents (e.g., checking for the exact filename and text), and output the next level password upon successful validation.