# Bandit Level 15 → Level 16

## Level Goal

The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

## Commands you may need to solve this level

ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Methodology
- For the password of next level, we have to submit the password of current level, bandit15, on port 30001 using SSL/TLS encryption this time.
- We can use `ncat`, `ncat` is basically `netcat` with native SSL/TLS and IPv6 support.
- After login as bandit15, 
```
bandit15@bandit:~$ 
```
- Basic enumeration, 
```
bandit15@bandit:~$ ls -al /home/bandit15
total 24
drwxr-xr-x   2 root     root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
-rw-r-----   1 bandit15 bandit15   33 Jun 24 14:59 .bandit14.password
-rw-r--r--   1 root     root      220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root     root      807 Feb 13 12:16 .profile
bandit15@bandit:~$ cat /home/bandit15/.bandit14.password 
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```
- We can now proceed with `ncat`, `ncat -h` for help.
```
bandit15@bandit:~$ ncat --ssl -v 127.0.0.1 30001
Ncat: Version 7.98 ( https://nmap.org/ncat )
Ncat: Subject: CN=SnakeOil
Ncat: Issuer: CN=SnakeOil
Ncat: SHA-1 fingerprint: 323A F3B1 4FC7 1B0F F71A 1931 8FF3 62A1 49AC 735A
Ncat: Certificate verification failed (self-signed certificate).
Ncat: SSL connection to 127.0.0.1:30001.
Ncat: SHA-1 fingerprint: 323A F3B1 4FC7 1B0F F71A 1931 8FF3 62A1 49AC 735A
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
Correct!
REDACTED

Ncat: 33 bytes sent, 43 bytes received in 17.86 seconds.
```
- We got the password of bandit16, next level.
## Key Takeaways / Concepts
* **SSL/TLS Network Connections (`ncat --ssl`):** Standard network tools like `nc` cannot communicate with ports running secure encryption layers. Using `ncat` with the `--ssl` flag (or `openssl s_client`) allows you to establish a secure connection over SSL/TLS to interact with encrypted services.
* **Handling Self-Signed Certificates:** Test and CTF servers frequently use self-signed certificates. While standard clients will throw verification warnings or errors, the connection can still successfully be established to pass or receive data.