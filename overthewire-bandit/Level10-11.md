# Bandit Level 10 → Level 11

## Level Goal

The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd
## Methodology
- First thing first, login as bandit10, after successful login
```
└─$ ssh bandit10@bandit.labs.overthewire.org -p 2220
```
- We need to read `data.txt`, that contains the base64 encoded data no issue, we can decode that using `base64 -d`
```
bandit10@bandit:~$ cat /home/bandit10/data.txt
VGhlIHBhc3N3b3JkIGlzIHBZZk9ZNkh3VXNEajVyTDlVdnloVTdNQ212OHZONVJvCg==
```
- Let's check if `base64` is installed or not in first place.
- If it isn't installed, we will use CyberChef.
```
bandit10@bandit:~$ base64 -h
encode/decode data and print to standard output
With no FILE, or when FILE is -, read standard input.

The data are encoded as described for the base64 alphabet in RFC 3548.
When decoding, the input may contain newlines in addition
to the bytes of the formal base64 alphabet. Use --ignore-garbage
to attempt to recover from any other non-alphabet bytes in the
encoded stream.

Usage: base64 [OPTION]... [FILE]

Arguments:
  [file]...  

Options:
  -d, --decode          decode data [aliases: -D]
  -i, --ignore-garbage  when decoding, ignore non-alphabetic characters
  -w, --wrap <COLS>     wrap encoded lines after COLS character (default 76, 0 to disable wrapping)
  -h, --help            Print help
  -V, --version         Print version
```
- `base64` is installed, we can proceed without any issue.
```
bandit10@bandit:~$ cat data.txt | base64 -d
The password is pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
bandit10@bandit:~$ 
```
- We got the password for next level, bandit11.
## Key Takeaways / Concepts
- We can encode or decode using with build-in tool `base64`