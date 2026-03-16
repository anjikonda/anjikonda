# 🐧 Chapter 03 — Linux for Hackers

> **Level:** Beginner | **Read time:** 25 minutes
> Linux is the operating system of cybersecurity. Almost every hacking tool runs on it.

---

## 📌 What You Will Learn

- Why hackers use Linux
- How to navigate the Linux file system
- 40+ essential commands with real examples
- File permissions — how to read and change them
- Users and groups
- Networking commands every hacker uses
- Bash scripting basics
- Useful shortcuts to work faster

---

## 1️⃣ Why Do Hackers Use Linux?

![Kali Linux terminal hacking environment](https://images.unsplash.com/photo-1629654297299-c8506221ca97?w=800&q=80)

Most beginners ask: "Why not just use Windows?"

Here is the honest answer:

```
Windows  → Built for everyday users — office work, gaming, browsing
Linux    → Built for power users — servers, development, security
Kali     → Linux specifically built for ethical hacking and penetration testing
```

**Reasons cybersecurity professionals use Linux:**

| Reason | Explanation |
|--------|------------|
| **Free and open source** | Anyone can read the source code and verify it |
| **All hacking tools run on it** | Nmap, Metasploit, Burp Suite, Wireshark — built for Linux |
| **Most servers run Linux** | When you hack or defend servers, they run Linux |
| **Complete control** | You can customise and automate everything |
| **Powerful command line** | You can do in 1 command what takes 10 mouse clicks in Windows |
| **Kali Linux** | Comes with 600+ hacking tools pre-installed |

> 💡 **Start with:** Install **Kali Linux** in a Virtual Machine (VirtualBox — free).
> This way you have Windows for everyday use AND Linux for learning — on the same computer.

---

## 2️⃣ The Linux File System

![Linux file system directory tree](https://images.unsplash.com/photo-1544197150-b99a580bb7a8?w=800&q=80)

Linux organises files in a tree structure. Everything starts from a single root: `/`

```
/  (root — the very top, like C:\ in Windows)
│
├── home/          → Personal files for each user
│   └── anji/      → YOUR personal folder (/home/anji)
│       ├── Desktop/
│       ├── Downloads/
│       └── Documents/
│
├── etc/           → System configuration files (very important!)
│   ├── passwd     → List of all user accounts
│   ├── shadow     → Hashed passwords (only root can read)
│   └── hosts      → Local DNS — maps hostnames to IPs
│
├── var/           → Variable data — changes constantly
│   └── log/       → Log files — all system activity recorded here
│       ├── syslog → General system log
│       └── auth.log → Login attempts — critical for security!
│
├── tmp/           → Temporary files — deleted on restart
│                    (Often used by attackers to store tools!)
│
├── bin/           → Basic programs everyone can use (ls, cat, ping)
├── sbin/          → System programs only root can use
├── usr/           → Installed software and libraries
│   └── bin/       → Most installed programs live here
│
├── root/          → Home folder for the root (admin) user
├── dev/           → Device files (hard drives, USB, etc.)
├── proc/          → Running processes and system information
└── mnt/           → Where you mount external drives
```

**The most important folders for security:**

```
/etc/passwd    → See all user accounts on the system
/etc/shadow    → Hashed passwords (hackers love this file)
/var/log/      → ALL activity logs — defenders monitor this
/tmp/          → Attackers often store files here (writable by everyone)
/root/         → The most powerful user's home — gaining access here = game over
```

---

## 3️⃣ Navigation Commands

These are the commands you will type hundreds of times every day.

### Moving Around

```bash
pwd
# Print Working Directory — shows exactly where you are right now
# Example output: /home/anji/Documents

ls
# List files and folders in current directory
# Example output: Desktop  Downloads  Documents  notes.txt

ls -l
# List with details — permissions, size, owner, date
# Example:
# -rw-r--r-- 1 anji anji 1024 Mar 15 10:30 notes.txt
# drwxr-xr-x 2 anji anji 4096 Mar 15 09:00 Documents

ls -la
# Same as ls -l but ALSO shows hidden files
# (Hidden files in Linux start with a dot, like .bashrc)

ls -lh
# Same as ls -l but shows file sizes in human-readable form (KB, MB, GB)

cd /home/anji
# Change Directory — go to /home/anji folder

cd Documents
# Go into the Documents folder (relative to where you are now)

cd ..
# Go one folder UP (back to parent folder)

cd ~
# Go directly to YOUR home folder (/home/anji) from anywhere

cd /
# Go to root — the very top of the file system

cd -
# Go back to the previous directory you were in
```

---

## 4️⃣ File and Folder Commands

### Creating Files and Folders

```bash
mkdir notes
# Make a new folder called "notes"

mkdir -p projects/cybersecurity/tools
# Make nested folders all at once
# Creates: projects/ → cybersecurity/ → tools/ (all in one command)

touch hello.txt
# Create an empty file called hello.txt
# If the file already exists, it just updates the timestamp

touch file1.txt file2.txt file3.txt
# Create multiple files at once
```

### Reading Files

```bash
cat notes.txt
# Display the entire contents of a file
# Good for short files

cat /etc/passwd
# Read the user accounts file
# (This is something hackers and pen testers check early)

less notes.txt
# Read a file page by page (good for long files)
# Use arrow keys to scroll, Q to quit

head notes.txt
# Show only the first 10 lines of a file

head -n 20 notes.txt
# Show the first 20 lines

tail notes.txt
# Show only the last 10 lines

tail -f /var/log/auth.log
# LIVE follow a log file — new lines appear as they are written
# (Security monitoring uses this constantly)
```

### Editing Files

```bash
nano notes.txt
# Open a file in nano — the simplest text editor
# Ctrl+O to save, Ctrl+X to exit

vim notes.txt
# Open in vim — more powerful but has a learning curve
# Press I to start typing, ESC then :wq to save and quit, :q! to quit without saving

echo "Hello World" > file.txt
# Write "Hello World" into file.txt (overwrites any existing content)

echo "New line" >> file.txt
# APPEND "New line" to the end of file.txt (does not overwrite)
```

### Copying, Moving, Deleting

```bash
cp notes.txt backup.txt
# Copy notes.txt and create a copy called backup.txt

cp -r myfolder/ myfolder_backup/
# Copy an entire folder and everything inside it (-r = recursive)

mv notes.txt renamed_notes.txt
# Rename a file (move it to a new name in the same location)

mv notes.txt /home/anji/Documents/
# Move notes.txt into the Documents folder

rm notes.txt
# Delete a file permanently (NO recycle bin in Linux — gone forever!)

rm -r myfolder/
# Delete an entire folder and everything inside it

rm -rf myfolder/
# Force delete — no confirmation asked, deletes everything
# BE VERY CAREFUL with this command
# Never run: rm -rf / (this deletes the entire operating system!)

rmdir emptyfolder/
# Delete a folder — only works if the folder is completely empty
```

---

## 5️⃣ Searching and Finding

```bash
find / -name "passwords.txt"
# Search the ENTIRE system for a file named "passwords.txt"
# This is one of the first things hackers do after getting access

find /home -name "*.txt"
# Find all .txt files inside /home

find / -name "*.conf" 2>/dev/null
# Find all configuration files, hide error messages
# (2>/dev/null sends error messages to nowhere — cleaner output)

find / -perm -4000 2>/dev/null
# Find all SUID files — files that run as root regardless of who runs them
# (Critical for privilege escalation — finding ways to become root)

grep "password" notes.txt
# Search INSIDE a file for the word "password"

grep -i "password" notes.txt
# Same but case-insensitive (finds Password, PASSWORD, password)

grep -r "password" /var/www/
# Search for "password" inside ALL files in /var/www/ recursively
# (Hackers use this to find hardcoded credentials in website code)

grep -n "error" logfile.txt
# Show line numbers where "error" appears

grep -v "INFO" logfile.txt
# Show all lines that do NOT contain "INFO" (invert the search)
```

---

## 6️⃣ File Permissions — The Access Control System

![Linux file permissions diagram](https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?w=800&q=80)

This is one of the most important concepts in Linux security. Every file and folder has a set of permissions that controls who can read, write to, or execute it.

### Reading Permission Output

When you run `ls -l`, you see something like this:

```
-rwxr-xr-- 1 anji staff 2048 Mar 15 10:30 script.sh
```

Let us break this down character by character:

```
- rwx r-x r--
│ │   │   │
│ │   │   └── Others (everyone else) — r-- = read only
│ │   └────── Group (staff group) — r-x = read and execute
│ └────────── Owner (anji) — rwx = read, write, execute
└──────────── File type: - = regular file, d = directory, l = symbolic link
```

### The Three Permission Types

| Letter | Permission | For Files | For Folders |
|--------|-----------|-----------|-------------|
| **r** | Read | Can view the file contents | Can list what is inside |
| **w** | Write | Can edit the file | Can create/delete files inside |
| **x** | Execute | Can run it as a program | Can enter (cd into) the folder |
| **-** | No permission | Cannot do that action | Cannot do that action |

---

### Changing Permissions with chmod

**Method 1 — Using letters (easier to understand):**

```bash
chmod +x script.sh
# Add execute permission for everyone

chmod -w notes.txt
# Remove write permission (make it read-only)

chmod u+x script.sh
# Add execute for the user (owner) only
# u = user/owner, g = group, o = others, a = all

chmod go-w privatefile.txt
# Remove write from group AND others
```

**Method 2 — Using numbers (faster once you learn it):**

Each permission has a number value:
```
r = 4
w = 2
x = 1

To get a permission set, add the numbers:
rwx = 4+2+1 = 7
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0
```

So `chmod 755 script.sh` means:
```
7 = rwx (owner can read, write, execute)
5 = r-x (group can read and execute)
5 = r-x (others can read and execute)
```

**Common permission combinations:**

```bash
chmod 755 script.sh   # Owner: full | Group: read+execute | Others: read+execute
chmod 644 notes.txt   # Owner: read+write | Group: read | Others: read (most common for files)
chmod 700 private/    # Owner: full | Group: none | Others: none (private folder)
chmod 777 shared/     # Everyone: full access (DANGEROUS — avoid in production!)
chmod 400 id_rsa      # SSH private key — owner read only (required by SSH)
```

---

### Changing Ownership with chown

```bash
chown anji notes.txt
# Change the owner of notes.txt to "anji"

chown anji:staff notes.txt
# Change owner to "anji" AND group to "staff"

chown -R anji:anji myfolder/
# Change owner and group for a folder AND everything inside it (-R = recursive)

sudo chown root:root /etc/important.conf
# Change owner to root (requires sudo)
```

---

### SUID — A Critical Security Concept

**SUID** (Set User ID) is a special permission that makes a file run as its owner — even when run by someone else.

```bash
ls -la /usr/bin/passwd
# Output: -rwsr-xr-x 1 root root ... passwd
#              ^
#              s = SUID is set

# The passwd command is owned by root and has SUID
# This means when ANY user runs passwd, it runs with root permissions
# (needed so regular users can change their own passwords)
```

**Why hackers love finding SUID files:**
If a program with SUID has a vulnerability, an attacker can exploit it to run commands as root — gaining full control of the system. This is called **privilege escalation**.

```bash
# Find all SUID files on a system (done during penetration tests)
find / -perm -4000 2>/dev/null
```

---

## 7️⃣ Users and Groups

```bash
whoami
# Show which user you are currently logged in as

id
# Show your user ID (uid), group ID (gid), and all groups you belong to
# Example: uid=1000(anji) gid=1000(anji) groups=1000(anji),27(sudo)

who
# Show who is currently logged into the system

w
# More detailed — shows logged in users and what they are doing

sudo command
# Run a command as root (administrator)
# sudo = SuperUser DO

sudo su
# Switch to root user completely (be careful — root can do anything)

su anji
# Switch to user "anji"

cat /etc/passwd
# List all users on the system
# Format: username:x:uid:gid:info:home:shell
# Example: anji:x:1000:1000:Anji,,,:/home/anji:/bin/bash

cat /etc/shadow
# List hashed passwords (requires root to read)
# This is what hackers try to access for offline password cracking

useradd newuser
# Create a new user (run as root)

passwd newuser
# Set a password for a user

usermod -aG sudo anji
# Add user "anji" to the sudo group (give admin privileges)
```

---

## 8️⃣ Networking Commands

These are the commands you will use most in cybersecurity work.

```bash
ifconfig
# Show all network interfaces and their IP addresses
# (Older command — still works on most systems)

ip a
# Modern version of ifconfig — shows all IP addresses
# Example output:
# inet 192.168.1.5/24 brd 192.168.1.255 scope global eth0

ip route
# Show the routing table — how your computer decides where to send traffic

ping google.com
# Send test packets to google.com and check if it responds
# Ctrl+C to stop
# Shows response time in milliseconds

ping -c 4 google.com
# Send exactly 4 pings then stop

traceroute google.com
# Show every router (hop) the packets pass through on the way to google.com
# Useful to understand network paths and find where traffic is blocked

nslookup google.com
# Find the IP address for a domain name (DNS lookup)

dig google.com
# More detailed DNS lookup
# dig google.com MX   → Find mail servers for the domain
# dig google.com ANY  → Find all DNS records

curl ifconfig.me
# Find your public IP address (what the internet sees)

wget https://example.com/file.zip
# Download a file from the internet

netstat -tulnp
# Show all open ports and what programs are using them
# t=TCP, u=UDP, l=listening, n=numbers not names, p=program name

ss -tulnp
# Modern replacement for netstat — same information, faster

nc -zv 192.168.1.1 1-1000
# Netcat — scan ports 1 to 1000 on a target
# (Simple port scanner)

nc -lvp 4444
# Netcat — listen for incoming connections on port 4444
# (Used for creating reverse shells in penetration testing)
```

---

## 9️⃣ Process Management

```bash
ps aux
# Show all running processes on the system
# a=all users, u=user format, x=processes without terminal
# This shows what programs are running — useful for finding malware

ps aux | grep apache
# Find if the apache web server is running

top
# Live view of running processes — updates every few seconds
# Shows CPU and memory usage
# Press Q to quit

htop
# Better version of top — colour coded, easier to read (may need to install)

kill 1234
# Stop the process with ID 1234

kill -9 1234
# Force kill the process (when normal kill doesn't work)

killall firefox
# Kill all processes named "firefox"

jobs
# Show background jobs in current terminal

Ctrl + C
# Stop the currently running command immediately

Ctrl + Z
# Pause the current command (send to background)

bg
# Resume the paused command in the background

fg
# Bring background command back to front
```

---

## 🔟 Bash Scripting Basics

![Terminal with bash script running](https://images.unsplash.com/photo-1515879218367-8466d910aaa4?w=800&q=80)

A bash script is a text file containing a list of commands. Instead of typing them one by one, you save them in a file and run them all at once. This is how security professionals automate repetitive tasks.

### Your First Bash Script

```bash
# Step 1: Create a new file
nano myscript.sh

# Step 2: Type this inside the file:
#!/bin/bash
# The first line #!/bin/bash tells Linux: "run this with bash"

echo "Hello! My name is Anji"
echo "Today is: $(date)"
echo "I am running as: $(whoami)"
echo "My IP address is: $(hostname -I)"

# Step 3: Save and exit (Ctrl+O, Enter, Ctrl+X)

# Step 4: Give it execute permission
chmod +x myscript.sh

# Step 5: Run it
./myscript.sh

# Output:
# Hello! My name is Anji
# Today is: Mon Mar 15 10:30:00 IST 2025
# I am running as: anji
# My IP address is: 192.168.1.5
```

---

### Variables

```bash
#!/bin/bash

name="Anji"
city="Hyderabad"
age=25

echo "My name is $name"
echo "I live in $city"
echo "I am $age years old"

# Storing command output in a variable
current_user=$(whoami)
echo "Running as: $current_user"
```

---

### If Statements

```bash
#!/bin/bash

ping -c 1 google.com > /dev/null 2>&1

if [ $? -eq 0 ]; then
    echo "Internet is working"
else
    echo "No internet connection"
fi

# $? contains the exit code of the last command
# 0 = success, anything else = failure
```

---

### Loops

```bash
#!/bin/bash

# Scan a range of IPs to see which ones are online
for i in {1..10}; do
    ping -c 1 192.168.1.$i > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "192.168.1.$i is ONLINE"
    else
        echo "192.168.1.$i is offline"
    fi
done
```

---

### A Real Security Script — Port Scanner

```bash
#!/bin/bash

target="192.168.1.1"
echo "Scanning $target for open ports..."
echo "================================"

for port in 21 22 23 25 53 80 110 143 443 3306 3389 8080; do
    (echo >/dev/tcp/$target/$port) 2>/dev/null
    if [ $? -eq 0 ]; then
        echo "Port $port is OPEN"
    fi
done

echo "Scan complete."
```

---

## 1️⃣1️⃣ Useful Shortcuts and Tips

### Keyboard Shortcuts

```
Ctrl + C    → Stop the running command immediately
Ctrl + Z    → Pause/suspend the current command
Ctrl + D    → Log out / exit current shell
Ctrl + L    → Clear the screen (same as typing 'clear')
Ctrl + A    → Jump to start of the command line
Ctrl + E    → Jump to end of the command line
Ctrl + U    → Delete everything to the left of cursor
Ctrl + K    → Delete everything to the right of cursor

Tab         → Auto-complete command or filename (press twice to see options)
↑ Arrow     → Go to previous command in history
↓ Arrow     → Go to next command in history
```

### History

```bash
history
# Show your last 500 commands

history | grep nmap
# Search your history for all times you used nmap

!!
# Repeat the very last command

!nmap
# Run the most recent command that started with "nmap"

Ctrl + R
# Reverse search — type a word and it finds the last command containing it
```

### Piping and Redirection — Powerful Combinations

```bash
# Pipe: take output of one command and send it as input to another
ls -la | grep ".txt"
# List all files, but only show lines containing ".txt"

ps aux | grep "apache"
# Show all processes, find if apache is running

cat /etc/passwd | cut -d: -f1
# Read passwd file, extract only the usernames (first field)

# Redirect: save command output to a file
nmap 192.168.1.1 > scan_results.txt
# Run nmap and save all output to scan_results.txt

nmap 192.168.1.1 >> all_scans.txt
# Append nmap output to the file (keeps previous content)

# Hide error messages
command 2>/dev/null
# 2> = redirect error output, /dev/null = the void (throws it away)

# Save both normal output and errors
command > output.txt 2>&1
```

---

## 1️⃣2️⃣ Package Management — Installing Tools

```bash
# Update the list of available packages
sudo apt update

# Upgrade all installed packages to latest versions
sudo apt upgrade

# Install a new tool
sudo apt install nmap
sudo apt install wireshark
sudo apt install hydra
sudo apt install gobuster

# Search for a package
apt search "password crack"

# Remove a package
sudo apt remove nmap

# Install multiple tools at once
sudo apt install nmap wireshark netcat hydra gobuster -y
```

---

## ✅ Quick Revision — Chapter 03

```
pwd         → Where am I? (current directory)
ls -la      → What files are here? (including hidden)
cd          → Change directory
cat         → Read a file
grep        → Search inside files
find        → Search for files by name or property
chmod       → Change file permissions
chown       → Change file ownership
ps aux      → See all running processes
netstat -tulnp → See all open ports
sudo        → Run as administrator
/etc/passwd → List of all users
/etc/shadow → Hashed passwords (root only)
/var/log/   → All system logs — critical for security
SUID        → Special permission — file runs as its owner
755         → Owner: full | Group: read+execute | Others: read+execute
644         → Owner: read+write | Group: read | Others: read
```

---

## 🔒 Security Tips — Apply Immediately

```
1. Never run commands as root unless absolutely necessary
2. Use sudo for individual commands instead of staying logged in as root
3. Check SUID files regularly: find / -perm -4000 2>/dev/null
4. Monitor auth logs: tail -f /var/log/auth.log
5. Close unnecessary open ports: netstat -tulnp
6. Keep software updated: sudo apt update && sudo apt upgrade
7. Strong passwords for all user accounts
8. Review /etc/passwd for any unexpected user accounts
```

---

## ➡️ Next Chapter

**[🕵️ Chapter 04 — Ethical Hacking Intro →]**

Learn the 5 phases of penetration testing, the methodology professionals follow, and the tools used to find and exploit vulnerabilities.

---

*📅 Written: March 2025 | 👤 Anji | Hyderabad, India*
*⭐ Star this repo if these notes helped you!*
