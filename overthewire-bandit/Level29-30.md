# Bandit Level 29 → Level 30

## Level Goal

There is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Commands you may need to solve this level

git
## Methodology

- For this level, we have to deal with Git, too.
- First we have to clone the required repo on our machine. with the password of bandit29.
```
└─$ git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
Cloning into 'repo'...
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit29-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
                                            
```
- Contents of cloned repo.
```
└─$ ls -l repo
total 4
-rw-rw-r-- 1 kali kali 131 Aug 16 02:33 README.md           
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ cd repo       
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ ls -al    
total 16
drwxrwxr-x 3 kali kali 4096 Aug 16 02:33 .
drwxrwxr-x 7 kali kali 4096 Aug 16 02:33 ..
drwxrwxr-x 7 kali kali 4096 Aug 16 02:33 .git
-rw-rw-r-- 1 kali kali  131 Aug 16 02:33 README.md    
```
- What's in `README.md`.
```
└─$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
  
```
- Nothing in `README.md`, looking into logs for useful commits.
```
└─$ git log --oneline 
a9c5d1c (HEAD, origin/master, origin/HEAD, master) fix username
b548c69 initial commit of README.md
```
- Nothing into commits, but we should check the commits, commit messages need not be explain all things that are committed in that commit. 
```
└─$ git switch --detach b548c69e5c7db
HEAD is now at b548c69 initial commit of README.md
                             
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit29
- password: <no passwords in production!>

┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ git switch --detach a9c5d1c2b4389080
Previous HEAD position was b548c69 initial commit of README.md
HEAD is now at a9c5d1c fix username
               
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>

```
- Nothing here for us, going deeper.
- We should look for changes that are not committed. using `git status`
```
└─$ git status              
HEAD detached at a9c5d1c
nothing to commit, working tree clean
```
- Nothing here, we are looking for loose branches now.
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo/.git]
└─$ cat packed-refs 
# pack-refs with: peeled fully-peeled sorted 
d36874ce7e88201c326bb596ba47a4cd063a023e refs/remotes/origin/dev
a9c5d1c2b43890809f3077bb9ec65c30ced242fb refs/remotes/origin/master
26abf6783ab7a4badd3931a31a1344b7cc961fa0 refs/remotes/origin/sploits-dev
```
- We have found some more commit IDs. Looking into `dev` branch.
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo/.git]
└─$ git log --oneline 
a9c5d1c (HEAD, origin/master, origin/HEAD, master) fix username
b548c69 initial commit of README.md

┌──(kali㉿kali)-[~/Desktop/overthewire/repo/.git]
└─$ git switch --detach d36874ce7e88201c326bb596ba47a4cd063a023e
fatal: this operation must be run in a work tree

┌──(kali㉿kali)-[~/Desktop/overthewire/repo/.git]
└─$ cd ../         

┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ git switch --detach d36874ce7e88201c326bb596ba47a4cd063a023e
Previous HEAD position was a9c5d1c fix username
HEAD is now at d36874c add data needed for development

```
- Now read the contents of `README.md`
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo]
└─$ cat README.md  
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX

```
- Okay, we got the password of bandit30, in this commit "`add data needed for development`", next level.
## Additional Work
- Other found branch is just distraction.
```
└─$ git switch --detach 26abf6783ab7a4badd3931a31a1344b7cc961fa0
Previous HEAD position was a9c5d1c fix username
HEAD is now at 26abf67 add some silly exploit, just for shit and giggles
```
- List of all remote branches.
```
└─$ git branch -r              
  origin/HEAD -> origin/master
  origin/dev
  origin/master
  origin/sploits-dev
                           
```
- Explanation of .git contents.

    - **.git/config**: Repo-specific configuration (e.g., remotes like origin, branches, merge settings).
    - **.git/description**: Short human-readable description/name of the repository.
    - **.git/HEAD**: Tells Git what to check out (usually points to a branch reference like refs/heads/main or a detached commit hash).
    - **.git/index**: The staging area (tracks what files/contents are staged for the next commit).
    - **.git/hooks/**: Optional local scripts Git runs at certain events (e.g., pre-commit, pre-push).
    - **.git/info/**: Local “exclude” rules not shared across clones (e.g., extra patterns for ignoring files).
    - **.git/logs/**: Reflog history (records movements of refs like HEAD and branch pointers over time).
    - **.git/objects/**: Database of stored Git content (commits, trees, blobs) and related metadata.
    - **.git/packed-refs**: Ref files that have been “packed” for efficiency (many refs stored in one file).
    - **.git/refs/**: References (names/branches/tags mapped to commit hashes), usually as files under heads/ (branches) and tags/ (tags).

## Key Takeaways / Concepts
- **Uncovering Hidden and Remote Branches:** When a secret is not present in the main branch history, checking for other branches (such as developer or feature branches via `git branch -a` or inspecting `.git/packed-refs`) often reveals hidden or forgotten code paths where credentials were committed.
- **Inspecting Git References (`packed-refs`):** Git optimizes storage by packing branch and tag references into files like `.git/packed-refs`, allowing you to identify historical or remote branch commit hashes that aren't immediately visible in a standard `git log` view.