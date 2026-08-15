# Linux Vault

A centralized personal knowledge base and repository for Linux administration notes, automation scripts, and CTF wargame write-ups.
## 📁 Vault Structure 
```
linux-vault/ 
├── README.md    <-- Main repository overview 
├── scripts/     <-- Bash automation scripts and utilities 
├── notes/       <-- General Linux concepts, commands, and cheat sheets 
└── overthewire-bandit/ <-- Walkthroughs and notes for OverTheWire: Bandit
```

## 🚀 Sections Overview

### 1. Scripts

- Collection of custom bash scripts and command-line utilities for system tasks and automation.
    

### 2. Linux Notes
- Core Linux concepts, privilege escalation notes, configuration references, and daily command-line cheatsheets.
    

### 3. OverTheWire: Bandit
Step-by-step walkthroughs, terminal methodologies, and key takeaways for the Bandit wargame series.

| Level Range        | Description / Key Focus                                                                                                                                                           | Walkthrough Link                                                                                  |
| :----------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| **Level 0**        | Introduction to wargames, bandit, login using (`ssh`)                                                                                                                             | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level0-0.md)   |
| **Level 0 to 1**   | Introduction to file searching (`find`) and exploring local directories                                                                                                           | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level0-1.md)   |
| **Level 1 to 2**   | Handing dashed filename (`-`), absolute & relative path                                                                                                                           | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level1-2.md)   |
| **Level 2 to 3**   | Working with filenames that contains whitespaces(`file name.txt `)                                                                                                                | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level2-3.md)   |
| **Level 3 to 4**   | Working with hidden files `...file-name` and `find -type d "dir-name"` to find directories                                                                                        | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level3-4.md)   |
| **Level 4 to 5**   | Identifying file types with `file` and handling dashed filenames via paths, leveraging wildcards (`*`)                                                                            | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level4-5.md)   |
| **Level 5 to 6**   | Finding files by properties using `find` (`-size`, `-not -executable`) and chaining commands with `-exec`                                                                         | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level5-6.md)   |
| **Level 6 to 7**   | Searching the entire filesystem with `find` using ownership (`-user`, `-group`) and size (`-size`) filters, while suppressing error messages via `2>/dev/null`                    | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level6-7.md)   |
| **Level 7 to 8**   | Searching through large text datasets and filtering lines for a specific keyword using `grep`                                                                                     | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level7-8.md)   |
| **Level 8 to 9**   | Finding unique lines in a dataset using command piping (`\|`), sorting with `sort`, and filtering non-duplicated entries via `uniq -u`                                            | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level8-9.md)   |
| **Level 9 to 10**  | Extracting human-readable text blocks from binary or obfuscated files using `strings` and filtering patterns with `grep`                                                          | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level9-10.md)  |
| **Level 10 to 11** | Decoding base64-encoded strings using the built-in `base64 -d` utility                                                                                                            | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level10-11.md) |
| **Level 11 to 12** | Decoding `ROT13` character rotation using CyberChef                                                                                                                               | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level11-12.md) |
| **Level 12 to 13** | Reversing hexdumps with `xxd -r`, working in secure sandboxes (`mktemp -d`), and iteratively decompressing nested archive formats (`gzip`, `bzip2`, `tar`)                        | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level12-13.md) |
| **Level 13 to 14** | Authenticating via SSH using a private key file (`-i`) and securing permissions (`chmod 600`)                                                                                     | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level13-14.md) |
| **Level 14 to 15** | Interacting with network socket services locally using `nc` (Netcat) to submit authentication strings                                                                             | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level14-15.md) |
| **Level 15 to 16** | Connecting to secure TLS/SSL-encrypted network ports locally using `ncat --ssl` to transmit credentials and retrieve the next password                                            | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level15-16.md) |
| **Level 16 to 17** | Scanning port ranges (`nmap -p1-100`) across localhost to find listening services, distinguishing SSL/TLS vs plain-text ports, and extracting SSH private key credentials         | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level16-17.md) |
| **Level 17 to 18** | Comparing file line-changes and spotting modifications between old and new datasets using the `diff` utility                                                                      | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level17-18.md) |
| **Level 18 to 19** | Bypassing automated shell logout scripts by executing a non-interactive command directly via SSH or spawning an alternate shell (`-t '/bin/sh'`)                                  | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level18-19.md) |
| **Level 19 to 20** | Leveraging SetUID binaries (`-rwsr-x---`) to execute privileged commands and read restricted files (`/etc/bandit_pass/`) as the owner user                                        | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level19-20.md) |
| **Level 20 to 21** | Working with network sockets, netcat (`nc`), and local port communication                                                                                                         | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level20-21.md) |
| **Level 21 to 22** | Inspecting cron jobs (`/etc/cron.d/`) and automated system scripts                                                                                                                | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level21-22.md) |
| **Level 22 to 23** | Analyzing script logic, variables (`whoami`), and hashed filenames (`md5sum`)                                                                                                     | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level22-23.md) |
| **Level 23 to 24** | Inspecting cron jobs (`/etc/cron.d/`) , permissions (`rwx`) (`421`)of files and folders and bash scripting                                                                        | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level23-24.md) |
| **Level 24 to 25** | Automating brute-force attacks with netcat (`nc`), bash loops, and filtering output via `grep -v`                                                                                 | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level24-25.md) |
| **Level 25 to 26** | Identifying custom login shells (`/usr/bin/showtext`), utilizing text viewer features like `more` and `vi` to escape restricted environments, and spawning a functional shell     | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level25-26.md) |
| **Level 26 to 27** | Leveraging custom shell breakout techniques (via pagers and text editors) combined with SetUID binaries (`bandit27-do`) to execute commands and read passwords as the target user | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level26-27.md) |
| **Level 27 to 28** | Cloning remote Git repositories over SSH with custom ports (`git clone ssh://user@host:port/...`) and authenticating using passwords                                              | [View Notes](https://github.com/rs-dot369/linux-vault/blob/main/overthewire-bandit/Level27-28.md) |
