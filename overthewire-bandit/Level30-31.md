# Bandit Level 30 → Level 31

## Level Goal

There is a git repository at `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Commands you may need to solve this level

git
## Methodology
- We have to clone the repo first.
```
└─$ git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo repo30-31 
Cloning into 'repo'...
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit30-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (4/4), done.                                
```
- Contents of repo30-31
```
└─$ cd repo30-31 

┌──(kali㉿kali)-[~/Desktop/overthewire/repo30-31]
└─$ ls -al 
total 16
drwxrwxr-x 3 kali kali 4096 Aug 16 06:54 .
drwxrwxr-x 8 kali kali 4096 Aug 16 06:57 ..
drwxrwxr-x 7 kali kali 4096 Aug 16 06:54 .git
-rw-rw-r-- 1 kali kali   30 Aug 16 06:54 README.md
```
- Let's read `README.md`
- I guess creator is teasing us.
```
└─$ cat README.md       
just an epmty file... muahaha 
```
- Looking into logs and remote branches.
```
└─$ git log --oneline
8f2cf5b (HEAD -> master, origin/master, origin/HEAD) initial commit of README.md

┌──(kali㉿kali)-[~/Desktop/overthewire/repo30-31]
└─$ git branch -r    
  origin/HEAD -> origin/master
  origin/master
```
- Let's look into `packed-refs` if it contains something useful for us.
```
└─$ cat .git/packed-refs                  
# pack-refs with: peeled fully-peeled sorted 
8f2cf5b700aa0dc83b8ec69f46974e81dd023e99 refs/remotes/origin/master
6a76bc87a774031428feb5cc910568293c335545 refs/tags/secret

┌──(kali㉿kali)-[~/Desktop/overthewire/repo30-31]
└─$ git switch --detach 6a76bc87a774031
fatal: unable to read tree (6a76bc87a774031428feb5cc910568293c335545)
```
- Checking what type of object is it, as `git switch --detach <hash>` expects that the object is a reachable commit whose tree can be read
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo30-31]
└─$ git cat-file -t 6a76bc87a774031428feb5cc910568293c335545
blob
```
- okay the hash is blob (raw content of the file, )not a commit (snapshot pointer for your project history).
- We can read blob's content. using `git cat-file -p <hash>`
```
┌──(kali㉿kali)-[~/Desktop/overthewire/repo30-31]
└─$ git cat-file -p 6a76bc87a774031428feb5cc910568293c335545
REDACTED
```
- We got the password of bandit31, next level.
## Additional Work
How they connect:
- Commit → Tree → Blob
    - A commit points to a tree.
    - A tree lists files and subdirectories and points to blobs for file contents.
    - Blobs are the actual file contents.
## Key Takeaways / Concepts
- **Git Object Types (`blob`, `tree`, `commit`, `tag`):** Under the hood, Git stores data as distinct object types. While commits store project history snapshots and trees represent directories, **blobs** contain the raw byte contents of individual files.
- **Inspecting Raw Git Objects (`git cat-file`):**
    - Use `git cat-file -t <hash>` to determine the underlying type of any Git object (commit, tree, blob, or annotated tag).
    - Use `git cat-file -p <hash>` to pretty-print and inspect the contents of that object directly, which is especially useful when dealing with tags or dangling blobs that aren't tied to standard branch history.