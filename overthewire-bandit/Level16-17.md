# Bandit Level 16 → Level 17

## Level Goal

The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

## Commands you may need to solve this level

ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Methodology
- At this level, we don't know the correct port, we have given a range, we use `nmap` for port scanning or we can write a simple port scanner in python.
- After successful login as bandit16, we can use `nmap` that is already installed.
```
bandit16@bandit:~$ nmap -T4 -sCV -p31000-32000 127.0.0.1
Starting Nmap 7.98 ( https://nmap.org ) at 2026-08-07 07:34 +0000
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00016s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
| ssl-cert: Subject: commonName=SnakeOil
| Not valid before: 2024-06-10T03:59:50
|_Not valid after:  2034-06-08T03:59:50
|_ssl-date: TLS randomness does not represent time
31691/tcp open  echo
31790/tcp open  ssl/unknown
|_ssl-date: TLS randomness does not represent time
| fingerprint-strings: 
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, LPDString, RTSPRequest, SIPOptions: 
|_    Wrong! Please enter the correct current password.
| ssl-cert: Subject: commonName=SnakeOil
| Not valid before: 2024-06-10T03:59:50
|_Not valid after:  2034-06-08T03:59:50
31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.98%T=SSL%I=7%D=8/7%Time=6A758AAB%P=x86_64-pc-linux-gn
SF:u%r(GenericLines,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cur
SF:rent\x20password\.\n")%r(GetRequest,32,"Wrong!\x20Please\x20enter\x20th
SF:e\x20correct\x20current\x20password\.\n")%r(HTTPOptions,32,"Wrong!\x20P
SF:lease\x20enter\x20the\x20correct\x20current\x20password\.\n")%r(RTSPReq
SF:uest,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20pass
SF:word\.\n")%r(Help,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu
SF:rrent\x20password\.\n")%r(FourOhFourRequest,32,"Wrong!\x20Please\x20ent
SF:er\x20the\x20correct\x20current\x20password\.\n")%r(LPDString,32,"Wrong
SF:!\x20Please\x20enter\x20the\x20correct\x20current\x20password\.\n")%r(S
SF:IPOptions,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x2
SF:0password\.\n");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 124.50 seconds
```
- We have found the correct port 31790, now we can submit the password of bandit16 to this port.
```
bandit16@bandit:~$ ncat --ssl 127.0.0.1 31790
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
Correct!
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

bandit16@bandit:~$ 
```
- We have private ssh key for login as bandit17, next level.
## Additional Work
- We can verify this private ssh key.
- Make a copy of this private ssh key, give necessary permission, `600`
```
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ ls -l bandit17_id_rsa   
-rw------- 1 kali kali 2602 Aug  7 03:31 bandit17_id_rsa
```

```
┌──(kali㉿kali)-[~/Desktop/overthewire]
└─$ ssh -i bandit17_id_rsa bandit17@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit17@bandit:~$ 
```
- Verified.
## Key Takeaways / Concepts 
* `-T4`: Sets the timing template to "Aggressive", speeding up the scan by adjusting parameters like retransmission timeouts. 
* `-sC`: Runs default Nmap Scripting Engine (NSE) scripts to gather extra information (such as SSH host keys, SSL certificates, or HTTP headers). 
* `-sV`: Enables version detection, probing open ports to determine what service and software version are actually running.
* `-p1-100`: Custom range of ports.