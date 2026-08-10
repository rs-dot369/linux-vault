# Bandit Level 4 → Level 5

## Level Goal

The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

## Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)
## Methodology
* Starting with login as bandit4.
```
└─$ ssh bandit4@bandit.labs.overthewire.org -p 2220
```
* Here we have to read a file in `inhere` directory.
* But here, we got 9 files to read and all names starts with '-'
```
bandit4@bandit:~$ ls -al /home/bandit4/inhere/
total 48
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file00
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file01
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file02
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file03
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file04
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file05
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file06
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file07
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file08
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file09
drwxr-xr-x 2 root    root    4096 Jun 24 14:59 .
drwxr-xr-x 3 root    root    4096 Jun 24 14:59 ..
```
* We can use this, to read files one by one.
```
bandit4@bandit:~$ more /home/bandit4/inhere/-*
::::::::::::::
/home/bandit4/inhere/-file00
::::::::::::::
��C�
    ������t!g����Ǔ�|0�a�E>d
::::::::::::::
/home/bandit4/inhere/-file01
::::::::::::::
�X&v{����M=�&�.����q��� Ɩp��G��
::::::::::::::
/home/bandit4/inhere/-file02
::::::::::::::
�e��tE�XQ�7�[��8�s�_"،�WW+��b�1

::::::::::::::
/home/bandit4/inhere/-file03
::::::::::::::
���M�����B�}�ȼ%�8�j���Ji��CH�L
::::::::::::::
/home/bandit4/inhere/-file04
::::::::::::::
▒�Ei��E��`�Y�k�q��3S/����9���
::::::::::::::
/home/bandit4/inhere/-file05
::::::::::::::
�P@�Kk����HJ�͡���<�M�J�2ϸ���
::::::::::::::
/home/bandit4/inhere/-file06
::::::::::::::
�{g�>������-�{�
               �!
                 0A�����F$\X�
::::::::::::::
/home/bandit4/inhere/-file07
::::::::::::::
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
::::::::::::::

```
* But this is not a good idea, what if we have 20 files or more files, this process it time taking.
* We can also use `file` to determine the types of files in `inhere directory` to determine the file which we need.
```
bandit4@bandit:~$ file /home/bandit4/inhere/*
/home/bandit4/inhere/-file00: data
/home/bandit4/inhere/-file01: data
/home/bandit4/inhere/-file02: OpenPGP Secret Key
/home/bandit4/inhere/-file03: data
/home/bandit4/inhere/-file04: data
/home/bandit4/inhere/-file05: data
/home/bandit4/inhere/-file06: Non-ISO extended-ASCII text, with NEL line terminators
/home/bandit4/inhere/-file07: ASCII text
/home/bandit4/inhere/-file08: data
/home/bandit4/inhere/-file09: data
```
* Now we can directly read the `/home/bandit4/inhere/-file07`, as ASCII encoding is human-readable.
```
bandit4@bandit:~$ cat /home/bandit4/inhere/-file07
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
```
* We got the password for next level, bandit5.
## Key Takeaways / Concepts
* **Identifying File Types:** In Linux, file extensions (like `.txt` or `.exe`) don't matter. The `file` command reads the file's contents (specifically its magic bytes) to tell you exactly what kind of data it contains, making it perfect for finding human-readable `ASCII text`.
* **The Power of Wildcards:** Using the asterisk wildcard (`*`) allows you to run commands on multiple files at once, such as running `file /home/bandit4/inhere/*` to scan the entire directory instantly.
* **Handling Dashed Filenames:** Commands like `cat` and `file` interpret arguments starting with a dash (`-`) as flags/options. We successfully bypassed this by using the absolute path (`/home/.../-file07`). Another common way to read these files is by using a relative path like `cat ./-file07` or using the double-dash `--` which tells the command to stop looking for flags (e.g., `cat -- -file07`).