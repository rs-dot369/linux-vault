# Linux Command Cheat Sheet

## 1. System Navigation & Basic Information

|**Command**|**Description**|
|---|---|
|`pwd`|Print current working directory path|
|`whoami`|Display the name of the logged-in user|
|`date`|Display current system date and time|
|`date +%D`|Display date in standard `MM/DD/YY` format|
|`ls`|List files and directories in current location|
|`ls -lt`|List files sorted by modification time (newest first)|
|`ls -la`|List all files, including hidden files (`.`) with details|
|`clear`|Clear the terminal screen|

## 2. File & Directory Management

### Creating, Viewing & Navigating

Bash

```
# View directory contents and paths
cd /path/to/folder    # Change directory
cd ..                 # Move back one directory level

# Create files and directories
touch filename.txt     # Create an empty file
mkdir my_folder        # Create a new directory

# View file contents
cat filename.txt       # Display full content on terminal
less filename.txt      # View file with pagination and search capability (/word)
more filename.txt      # View file page-by-page
head -n 5 filename.txt # Read top 5 lines of a file
tail -n 5 filename.txt # Read bottom 5 lines of a file
```

### Copying, Moving & Deleting

Bash

```
# Copying
cp file.txt /dest/path/            # Copy file to another directory
cp fileA.txt fileB.txt              # Copy content to a new file

# Moving & Renaming
mv file.txt /dest/path/            # Move (cut/paste) file to a folder
mv fileA.txt fileNewName.txt        # Rename a file

# Deleting
rm file.txt                        # Delete a file
rmdir my_folder                    # Delete an empty directory
rm -rf my_folder                   # Force delete a non-empty directory
```

## 3. Text Processing & Manipulation

Bash

```
# Sorting and Unique Lines
sort file.txt                      # Sort lines alphabetically
sort -r file.txt                   # Sort lines in reverse order
sort file.txt | uniq               # Display unique lines (requires sorted input)

# Searching and Filtering
grep "word" file.txt               # Search for matching lines containing "word"
egrep "word1|word2" file.txt       # Search for multiple pattern matches

# Line & File Modification
split -l 3 file.txt                # Split file into smaller files of 3 lines each
shuf file.txt                      # Shuffle lines randomly
wc -l file.txt                     # Count total number of lines
cmp fileA.txt fileB.txt            # Check if two files are identical
diff -u fileA.txt fileB.txt        # Compare and display differences between files

# File Stream Editing
awk -F ',' '{print $2}' file.csv   # Print 2nd column from a CSV file
cut -c1-2 file.txt                 # Display first 2 characters of every line
sed -n '5p' file.txt               # Display line number 5
sed -i 's/from/to/g' file.txt      # Replace "from" with "to" throughout file

# Text Transformation
tr '[:lower:]' '[:upper:]' < file.txt  # Convert text to uppercase
tr '[:punct:]' 'Z' < file.txt          # Replace punctuation marks with 'Z'
tr '[:digit:]' 'Z' < file.txt          # Replace numbers with 'Z'
fold -w1 <<< "ABCDE"               # Display string vertically (1 char per line)
truncate -s 100M file.txt          # Expand or shrink file size to 100MB
```

## 4. Wildcards & Searching Files

Bash

```
# Wildcard examples
ls file*                           # Match files starting with "file"
touch file{1..5}.txt               # Create file1.txt through file5.txt

# Searching for files
find /path/ -name "filename.txt"   # Search for file by name in a specific path
updatedb                           # Update the file location database
locate filename.txt                # Fast file search using database
```

## 5. System Utilities & Info

Bash

```
history                            # Show previously executed commands
man <command>                      # Open manual for a command
<command> --help                   # Display help and available options
which <command>                    # Show path of executable binary
bc                                 # Launch command-line calculator
cal                                # Display current calendar
uptime                             # Display how long system has been running
script activity.log                # Record terminal session (Exit with Ctrl+D) or exit
alias l="ls -ltr"                  # Create shortcut alias for long commands
```

## 6. Compression & Archives (`zip`, `tar`, `gzip`)
#### Gzip (.gz)
- Function: Compresses a single file for smaller size.
-  Compress: gzip file (creates file.gz)
- Decompress: gzip -d file.gz (or gunzip file.gz)
#### Bzip2 (.bz2)
- Function: Compresses a single file with higher compression than gzip (smaller size, slower speed).
- Compress: bzip2 file (creates file.bz2)
- Decompress: bzip2 -d file.bz2 (or bunzip2 file.bz2)
#### Tar (.tar)
- Function: Bundles multiple files and folders into one archive (does not compress by itself).
-  Create Archive: tar -cf archive.tar folder/
- Extract Archive: tar -xf archive.tar

Bash

```
# Gzip
gzip -k file.txt                   # Compress file (keep original)
gzip -d file.txt.gz                # Decompress .gz file
gunzip file.txt.gz                 # Alternative decompress command

# Tar (.tar.gz)
tar -czf myfiles.tar.gz folder/    # Compress folder into tarball
tar -xzf myfiles.tar.gz            # Extract tarball contents

# Zip
zip myfiles.zip file1 file2        # Zip multiple files
unzip -l myfiles.zip               # List contents of zipped archive without extracting
unzip myfiles.zip                  # Extract zip file
```

## 7. Package Management & Web Tools

Bash

```
# Downloading and Web Requests
wget URL_of_file                   # Download file from web
wget -O output.txt URL_of_file     # Download and save with specific filename
curl http://numbersapi.com/random  # Transfer data / Call API from terminal

# Installation & Packages (Debian/Ubuntu vs RHEL/CentOS)
sudo apt update && sudo apt upgrade             # Update package lists and upgrade apps
sudo apt install <package>                      # Install app on Debian/Ubuntu
sudo dnf install <package>                      # Install app on Fedora/RHEL

# Query Installed Packages
dpkg -l | grep app                              # Check installed apps (Debian/Ubuntu)
rpm -qa | grep app                              # Check installed apps (RHEL/CentOS)
dnf list installed                              # List all installed packages
```

## 8. Environment Variables & Services

Bash

```
printenv                           # List all active environment variables
export JAVA_HOME="/usr/lib/jvm/java" # Set temporary environment variable
export PATH=$JAVA_HOME/bin:$PATH   # Add directory to PATH variable
source ~/.bashrc                   # Reload configuration file permanently

# Managing System Services
systemctl start service_name       # Start a service
systemctl stop service_name        # Stop a service
systemctl list-units --type=service --all  # List all registered services
```

## 9. Permissions & User Management

### User Switching & Sudo

Bash

```
su username                        # Switch to another user
exit                               # Logout from current user session
sudo <command>                     # Execute command with root privileges
```

### Permissions (`chmod`, `chown`)

Bash

```
ls -ltr                            # View permissions (e.g., rwxr-xr--)
chmod a+rwx file.txt               # Grant read/write/execute to All (u=user, g=group, o=others)
chown root file.txt                # Change file owner to root
chgrp devgroup file.txt            # Change file group ownership
```

### User Administration

Bash

```
useradd username                   # Create a new user
passwd username                    # Set/Change password for user
groupadd groupname                 # Create a new group
id username                        # Display UID and GID details
userdel username                   # Delete a user
groupdel groupname                 # Delete a group
```

## 10. Process Management & Automation

Bash

```
ps -ef | grep java                 # Check if process is running
pgrep process_name                 # Get PID (Process ID) of a running process
kill -9 <PID>                      # Force stop a process using PID
pkill process_name                 # Stop a process using its name

# Background & Foreground Jobs
jobs                               # Display active background jobs
bg                                 # Resume paused job in background
fg                                 # Bring background job to foreground
nohup ./script.sh > /dev/null &    # Run script in background immune to hangups

# Scheduling Tasks
at 10:30 PM                        # Schedule one-time task
crontab -e                         # Edit recurring scheduled cron jobs
```

## 11. Hardware, Memory & System Status

Bash

```
# Memory & Disk Usage
free -h                            # Display free and used RAM (human-readable)
top                                # Real-time process and resource monitor
du -sh /path                       # Display disk usage of a directory
df -h                              # Display available disk space on file systems

# System Hardware Info
hostname                           # Print server hostname
lscpu                              # Display CPU details and architecture
arch                               # Display architecture type (e.g., x86_64)
lsblk                              # List block devices and storage partitions
uname -a                           # Display OS kernel name and release info
```

## 12. Networking & Remote Access

Bash

```
ifconfig                           # Display active network interfaces & IP addresses
ping google.com                    # Test network connectivity
telnet IP Port                     # Check if remote IP and port are reachable
netstat -putan | grep :80          # Check active connections on port 80
traceroute google.com              # Trace packet route to destination host

# Remote Access & Transfers
ssh user@hostname                  # Connect to remote server securely
scp file.txt user@hostname:/tmp/   # Securely copy file to remote server
```

## 13. Security & System Control

Bash

```
# Firewall Configuration
firewall-cmd --list-all            # View active firewall rules
firewall-cmd --add-port=2001/tcp   # Temporarily open TCP port 2001

# System Power Controls
reboot                             # Restart the server
shutdown -h now                    # Power off the server immediately
```