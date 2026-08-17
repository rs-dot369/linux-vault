# Bandit Level 14 → Level 15

## Level Goal

The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

## Commands you may need to solve this level

ssh, telnet, nc, openssl, s_client, nmap

## Methodology
- For the next level password, we have to submit password of bandit14 to port 30000 on localhost 127.0.0.1
- To connect on port 30000, we can use `netcat`(`nc`) or `telnet`
```
bandit14@bandit:~$ nc -v 127.0.0.1 30000
Connection to 127.0.0.1 30000 port [tcp/*] succeeded!
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
Correct!
REDACTED


bandit14@bandit:~$ 
```
- We have now password of bandit15, next level.
## Additional Work
```
bandit14@bandit:~$ ls -al /home/bandit14
total 24
drwxr-xr-x   3 root root 4096 Jun 24 14:59 .
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
-rw-r--r--   1 root root  220 Feb 13 12:16 .bash_logout
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root root  807 Feb 13 12:16 .profile
drwxr-xr-x   2 root root 4096 Jun 24 14:59 .ssh
bandit14@bandit:~$ ls -al /home/bandit14/.ssh/
total 12
drwxr-xr-x 2 root     root     4096 Jun 24 14:59 .
drwxr-xr-x 3 root     root     4096 Jun 24 14:59 ..
-rw-r----- 1 bandit14 bandit14  568 Jun 24 14:59 authorized_keys
```
## Key Takeaways / Concepts 
 * **Local Network Interaction (`nc` / Netcat):** The `nc` (Netcat) utility allows you to connect to TCP/UDP ports on a specific host (like `localhost` or `127.0.0.1`) to send data, interact with network services, or test open ports.
 * **Socket Communication:** Many challenges require piping data or manually typing a password into a running service listening on a specific port to retrieve the next flag.