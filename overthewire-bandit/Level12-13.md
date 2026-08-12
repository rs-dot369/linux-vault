# Bandit Level 12 → Level 13

## Level Goal

The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

## Methodology
- We need to read `data.txt` that is hexdump of a file, we can use `xxd` to read that.
- We need to login as `bandit12` first.
```
└─$ ssh bandit12@bandit.labs.overthewire.org -p 2220
```
- Contents of `/home/bandit12`
```
bandit12@bandit:~$ ls -al /home/bandit12/
total 24
drwxr-xr-x   2 root     root     4096 Jun 24 14:58 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
-rw-r-----   1 bandit13 bandit12 2641 Jun 24 14:58 data.txt
bandit12@bandit:~$ more /home/bandit12/data.txt 
00000000: 1f8b 0808 a6f0 3b6a 0203 6461 7461 322e  ......;j..data2.
00000010: 6269 6e00 0144 02bb fd42 5a68 3931 4159  bin..D...BZh91AY
00000020: 2653 5904 ab91 e100 001c 7fff fffb bebf  &SY.............
00000030: f1fb dfbb be7f f57d fef5 5f8f ffcd b7b6  .......}.._.....
00000040: 19ff f6df af7f feae fff6 7fff 3001 3b6d  ............0.;m
...
...
...
```
- Using `xxd -r` to convert hexdump back into binary data.
```
bandit12@bandit:~$ mktemp -d
/tmp/tmp.QfX957uV0L
bandit12@bandit:~$ cd /tmp/tmp.Qfx957uV0L
-bash: cd: /tmp/tmp.Qfx957uV0L: No such file or directory
bandit12@bandit:~$ cd /tmp/tmp.QfX957uV0L
bandit12@bandit:/tmp/tmp.QfX957uV0L$ xxd -r /home/bandit12/data.txt > binary.bin
bandit12@bandit:/tmp/tmp.QfX957uV0L$ more binary.bin 
��;jdata2.binD��BZh91AY&SY����������߻��}��_��ͷ���߯�����0;m[▒4▒4�
...
```
- This is not human readable, we have to determine its `true file type` by examining its internal structure. Using `file`,
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file binary.bin 
binary.bin: gzip compressed data, was "data2.bin", last modified: Wed Jun 24 14:58:46 2026, max compression, from Unix, original size modulo 2^32 580
```
- Look here, `data2.bin` is compressed into `binary.bin`, we have to rename it first, `.gz`, to extract it we  can use `gzip -d`
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv binary.bin binary.gz
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 4
-rw-rw-r-- 1 bandit12 bandit12 613 Aug 12 05:56 binary.gz
bandit12@bandit:/tmp/tmp.QfX957uV0L$ gzip -d binary.gz 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -al
total 4
drwx------  2 bandit12 bandit12   60 Aug 12 06:00 .
drwxrwx-wt 57 root     root     1460 Aug 12 06:00 ..
-rw-rw-r--  1 bandit12 bandit12  580 Aug 12 05:56 binary
```
- We have to just follow the sequence by determining file type. Then `bzip2 -d`
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file binary 
binary: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv binary binary.bz2
bandit12@bandit:/tmp/tmp.QfX957uV0L$ bzip2 -d binary.bz2 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 4
-rw-rw-r-- 1 bandit12 bandit12 438 Aug 12 05:56 binary
```
- Again determine the file type, then follow. 
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file binary 
binary: gzip compressed data, was "data4.bin", last modified: Wed Jun 24 14:58:46 2026, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv binary binary.gz
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 4
-rw-rw-r-- 1 bandit12 bandit12 438 Aug 12 05:56 binary.gz
bandit12@bandit:/tmp/tmp.QfX957uV0L$ gzip -d binary.gz 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 20
-rw-rw-r-- 1 bandit12 bandit12 20480 Aug 12 05:56 binary
```
- Determine the file type of output file, `binary` and change its extension and use use `tar -xf`
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file binary 
binary: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv binary binary.tar
bandit12@bandit:/tmp/tmp.QfX957uV0L$ tar -xf binary.tar 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 32
-rw-rw-r-- 1 bandit12 bandit12 20480 Aug 12 05:56 binary.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data5.bin
```
- Just follow the next compression techniques.
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv data5.bin data5.tar
bandit12@bandit:/tmp/tmp.QfX957uV0L$ tar -xf data5.tar 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls  -al
total 36
drwx------  2 bandit12 bandit12   100 Aug 12 06:05 .
drwxrwx-wt 58 root     root      1540 Aug 12 06:05 ..
-rw-rw-r--  1 bandit12 bandit12 20480 Aug 12 05:56 binary.tar
-rw-r--r--  1 bandit12 bandit12 10240 Jun 24 14:58 data5.tar
-rw-r--r--  1 bandit12 bandit12   223 Jun 24 14:58 data6.bin
```

```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv data6.bin data6.bz2
bandit12@bandit:/tmp/tmp.QfX957uV0L$ bzip2 -d data6.bz2 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 44
-rw-rw-r-- 1 bandit12 bandit12 20480 Aug 12 05:56 binary.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data5.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data6
```

```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file data6
data6: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv data6 data6.tar
bandit12@bandit:/tmp/tmp.QfX957uV0L$ tar -xf data6.tar 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 48
-rw-rw-r-- 1 bandit12 bandit12 20480 Aug 12 05:56 binary.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data5.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data6.tar
-rw-r--r-- 1 bandit12 bandit12    79 Jun 24 14:58 data8.bin
```

```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file data8.bin 
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jun 24 14:58:46 2026, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/tmp.QfX957uV0L$ mv data8.bin data8.gz
bandit12@bandit:/tmp/tmp.QfX957uV0L$ gzip -d data8.gz 
bandit12@bandit:/tmp/tmp.QfX957uV0L$ ls -l
total 48
-rw-rw-r-- 1 bandit12 bandit12 20480 Aug 12 05:56 binary.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data5.tar
-rw-r--r-- 1 bandit12 bandit12 10240 Jun 24 14:58 data6.tar
-rw-r--r-- 1 bandit12 bandit12    49 Jun 24 14:58 data8
```
- Finally, we have human readable format, `ASCII` format.
```
bandit12@bandit:/tmp/tmp.QfX957uV0L$ file data8 
data8: ASCII text
bandit12@bandit:/tmp/tmp.QfX957uV0L$ cat data8 
The password is qQYQiHOBPR8zR61qxYqX45quvihF2uzk
```
- We got the password for next level, bandit13.
## Key Takeaways / Concepts
* **Reversing Hex Dumps (`xxd -r`):** The `xxd` command in reverse mode (`-r`) translates a hexadecimal text dump back into raw binary bytes, allowing you to restore obfuscated files to their original state. 
* **Safe Working Directories (`mktemp -d`):** Creating a temporary directory via `mktemp -d` provides a secure, isolated sandbox inside `/tmp` with a randomized name, preventing permission errors and keeping your workspace clean. 
* **Iterative File Analysis (`file`):** When dealing with repeatedly compressed or archived files, extensions are often stripped. Using the `file` command at each step reveals the underlying structure (e.g., gzip, bzip2, tar) so you know which extraction utility (`gzip -d`, `bzip2 -d`, `tar -xf`) to apply next.

Notes:
• Gzip (.gz)
- Function: Compresses a single file for smaller size.
-  Compress: gzip file (creates file.gz)
- Decompress: gzip -d file.gz (or gunzip file.gz)
• Bzip2 (.bz2)
- Function: Compresses a single file with higher compression than gzip (smaller size, slower speed).
- Compress: bzip2 file (creates file.bz2)
- Decompress: bzip2 -d file.bz2 (or bunzip2 file.bz2)
• Tar (.tar)
- Function: Bundles multiple files and folders into one archive (does not compress by itself).
-  Create Archive: tar -cf archive.tar folder/
- Extract Archive: tar -xf archive.tar





