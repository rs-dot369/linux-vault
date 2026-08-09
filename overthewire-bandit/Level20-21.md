# Bandit Level 20 → Level 21 Write-up

## Level Goal
There is a setuid binary in the home directory (`suconnect`) that connects to `localhost` on a specified port, reads a line of text, and compares it to the previous level's password. If correct, it outputs the password for the next level (`bandit21`).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

## Commands you may need to solve this level

ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)
## Solution Steps

### 1. Set up a local netcat listener
Start a background listener on a port (e.g., `8080`) that pipes the current level's password (`bandit20`) when a connection is made:

```bash
echo '4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA' | nc -l 8080 &
```

### 2. Run the setuid binary

Execute the `suconnect` binary pointing to that same port:

Bash

```
./suconnect 8080
```

### 3. Output & Result

Plaintext

```
Read: 4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
Password matches, sending next password
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```

## Key Takeaways / Concepts

- **Setuid binaries:** `suconnect` runs with the permissions of the file owner (`bandit21`), allowing it to read restricted data when triggered properly.
    
- **Job Control:** Using `&` or background processing (`CTRL+Z` -> `bg`) allows you to run a netcat listener concurrently while executing the binary in the active shell.

