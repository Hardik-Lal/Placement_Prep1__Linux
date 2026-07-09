# Placement_Prep_Linux
Here I would be uploading all my learning that is important for the upcoming college placements 
# Linux Notes — From Basics to Advanced 🐧

A structured set of notes covering Linux fundamentals, architecture, and the commands you actually use day to day — from how the internet works, down to volume management. Written while learning, organized here so others can learn too.

## 📑 Table of Contents

- [How the Internet Works](#how-the-internet-works)
- [Linux Architecture](#linux-architecture)
- [Basic File & System Commands](#basic-file--system-commands)
- [Links: Soft Link vs Hard Link](#links-soft-link-vs-hard-link)
- [Process & Session Commands](#process--session-commands)
- [System Level Commands](#system-level-commands)
- [Package Managers by Distro](#package-managers-by-distro)
- [User & Group Management](#user--group-management)
- [File Permissions](#file-permissions)
- [Compression Commands](#compression-commands)
- [File Transfer Commands](#file-transfer-commands)
- [Networking Commands](#networking-commands)
- [Text Processing Commands](#text-processing-commands)
- [Linux Volume Management (LVM)](#linux-volume-management-lvm)
- [Vim vs Nano](#vim-vs-nano)
- [Contributing](#contributing)

---

## How the Internet Works

- **ISP (Internet Service Provider)** — e.g. Jio — provides your device with an IP address and connects it to the wider internet.
- Your device connects to the ISP → the ISP connects to a **Data Center** via **optic fiber**.
- **Server**: A machine that *serves* information to clients that *request* it.

### Web Server vs Application Server

| | Web Server | Application Server |
|---|---|---|
| Stores | Static data (HTML, CSS, images) | Dynamic data |
| Computation | Very low computation required | High computation required |
| Examples | Nginx, Apache HTTP Server | Tomcat, Gunicorn, Node.js apps |

### Types of Applications

- **Standalone Application** — does not require the internet or any other app to function.
- **Web Application** — requires the internet and typically depends on many supporting services (databases, APIs, auth servers, etc.).

---

## Linux Architecture

- **Linux is an Operating System (OS).**

| Component | Description |
|---|---|
| **Kernel** | The core of the OS. It runs the essential processes required to keep the OS running — manages hardware, memory, processes, and system calls. |
| **Shell** | The interface/terminal through which the user communicates with the kernel (e.g. bash, zsh). |
| **Bootloader** | The process that loads the OS files into memory and starts the kernel. Example: **GRUB**. |

### Layered View

```
Shell
 └── Kernel
```

### Full Architecture (outside → in)

```
Application
 └── Shell
      └── Kernel
           └── Hardware
```

Each layer only talks to the one directly beneath it — applications don't talk to hardware directly, they go through the shell → kernel → hardware chain.

---

## Basic File & System Commands

| Command | Purpose |
|---|---|
| `top` | Real-time CPU usage / running processes |
| `df -h` | Hard disk usage (human-readable) |
| `free -h` | RAM usage (human-readable) |
| `touch <file>` | Create a new empty file |
| `cat <file>` | Display the contents of a file |
| `head <file>` | Display the top lines of a file (default 10) |
| `tail <file>` | Display the bottom lines of a file (default 10) |
| `pwd` | Print working directory |
| `cp <source> <destination>` | Copy a file from source to destination |
| `wc <file>` | Word/line/character count of a file |

> 💡 Extra worth knowing: `head -n 20 file` / `tail -n 20 file` to control how many lines are shown, and `tail -f file` to follow a file live (great for logs).

---

## Links: Soft Link vs Hard Link

| Type | Command | Behavior |
|---|---|---|
| **Soft Link (Symlink)** | `ln -s <file_location> <softlink-file>` | A shortcut/pointer to the original file's location. **Breaks (dangles)** if the source file is deleted. |
| **Hard Link** | `ln <file_path> <hardlink-file>` | Points directly to the same underlying data (inode). **Does not break** when the source is deleted — data persists as long as one link exists. |

---

## Process & Session Commands

| Command | Purpose |
|---|---|
| `ssh` | **S**ecure **Sh**ell — connects your system to a remote computer/server over a network |
| `nohup <command>` | Runs a command immune to hangups (e.g. terminal closing) and saves its output. Example: `nohup free -h` saves output to `nohup.out` |
| `vmstat` | Reports on virtual memory, processes, and CPU activity |
| `fuser` | Identifies which process is using a file, directory, or network port |
| `kill` | Stops or terminates a running process (by PID) |

**SSH Keys — 2 types:**
- **Private Key** → stays with the user
- **Public Key** → shared with/stored on the server

> 💡 Extra worth knowing: `kill -9 <pid>` force-kills a process; `kill -l` lists all available signals. `ssh-keygen` generates a new key pair.

---

## System Level Commands

| Command | Purpose |
|---|---|
| `uname` | Displays info about the OS and kernel (`uname -a` for full details) |
| `uptime` | Displays how long the system has been running since the last reboot |
| `whoami` | Tells you the current logged-in user |
| `sudo` | Runs a command with **admin (superuser) permissions** — the user must belong to the sudo/wheel group |
| `shutdown` | Shuts down the system (usually run with `sudo`) |
| `apt` | Application package manager for Linux (Debian/Ubuntu) — used to install/update applications |

---

## Package Managers by Distro

All of these are package managers — they just belong to different Linux distributions/platforms:

| Package Manager | Distribution |
|---|---|
| `apt` | Debian / Ubuntu |
| `yum` / `dnf` | CentOS / RHEL |
| `dnf` | Fedora |
| `pacman` | Arch Linux |
| `portage` (`emerge`) | Gentoo Linux |
| `rpm` | Red Hat |

---

## User & Group Management

| Command | Purpose |
|---|---|
| `useradd <user>` | Add a new user |
| `passwd <user>` | Change the password for a user |
| `su <user>` | Switch user |
| `userdel <user>` | Delete a user |
| `groupadd <group>` | Add a group |
| `groupdel <group>` | Delete a group |

> 💡 Extra worth knowing: `usermod -aG <group> <user>` adds an existing user to a group without recreating them; `id <user>` shows a user's UID, GID, and group memberships.

---

## File Permissions

Permission string format: `d rwx rwx rwx` → **[type][user][group][others]**

- `d` = directory (`-` for a regular file)
- Each `rwx` block = permissions for **User / Group / Others**

Example: `chmod 775 <filename>`

### Permission Values

| Permission | Value |
|---|---|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

### Full Permission Table

| Value | Read | Write | Execute |
|---|---|---|---|
| 0 | – | – | – |
| 1 | – | – | x |
| 2 | – | w | – |
| 3 | – | w | x |
| 4 | r | – | – |
| 5 | r | – | x |
| 6 | r | w | – |
| 7 | r | w | x |

For a folder/file: `chmod 777 <filename>` → **User / Group / Others**, each digit is the sum of the permission values you want to grant.

| Command | Purpose |
|---|---|
| `umask` | A 4-digit octal number used to determine the default permissions for newly created files |
| `chown` | Used to change ownership of a file (user) |
| `chgrp` | Used to change the group of a file |

> 💡 Extra worth knowing: `chown user:group file` changes both owner and group in one command. Default umask is usually `022`, which is why new files typically get `644` and new directories `755`.

---

## Compression Commands

| Command | Purpose |
|---|---|
| `zip` | Compress a file or list of files |
| `unzip` | Decompress/expand a `.zip` file |
| `tar` | Also used to compress/archive files (commonly with flags like `-czvf` to create, `-xzvf` to extract) |

---

## File Transfer Commands

| Command | Purpose |
|---|---|
| `scp` | Transfers files from local to remote desktop and vice versa. Syntax: `scp -i <private_key_path> <source> <destination>` |
| `rsync` | Syncs files between local and remote desktop (only transfers the differences). Syntax: `rsync -avz -e "ssh -i <private_key_path>" <source> <destination>` |

---

## Networking Commands

| # | Command | Purpose |
|---|---|---|
| 1 | `netstat` | Displays network connections, routing tables, interface statistics, masquerade connections, and multicast memberships |
| 2 | `traceroute` | Traces the path a packet takes from your PC to a destination host |
| 3 | `mtr` | "My traceroute" — combines both `traceroute` and `ping` |
| 4 | `nslookup` | Queries DNS to find the IP associated with a domain |
| 5 | `telnet` | Connects to a remote host using the telnet protocol, or tests whether a TCP port is open |
| 6 | `hostname` | Displays or changes the system's hostname |
| 7 | `iwconfig` | Configures and displays wireless network interfaces |
| 8 | `arp` | Displays or modifies the ARP table — **A**ddress **R**esolution **P**rotocol maps IP addresses to MAC addresses |
| 9 | `dig` | Performs detailed DNS queries |
| 10 | `whois` | Retrieves registration info about a domain |
| 11 | `curl` | Gets the response from a server — generally used for API calls |
| 12 | `wget` | Downloads anything publicly available on the internet |
| 13 | `iptables` | Linux firewall management tool |
| 14 | `nmap` | Scans hosts, discovers devices, and checks open ports |
| 15 | `route` | Displays or modifies the IP routing table |

> 💡 Extra worth knowing: `ping <host>` (basic reachability check) is the classic starting point before reaching for `mtr` or `traceroute`. `ss` is the modern replacement for `netstat` on most current distros.

---

## Text Processing Commands

| Command | Purpose |
|---|---|
| `awk` | Powerful text-processing and pattern-scanning tool used to search, filter, and manipulate **structured data** (works column by column). Syntax: `awk 'code' filename` (code = pattern/format, filename = text file or command output) |
| `sed` | Command-line utility used to search, replace, insert, delete, and edit text in files or command output. Works on **unstructured data** (any data), line by line — but doesn't modify output in place unless told to (needs `-i` to edit in place). Syntax: `sed 'code' filename` |
| `grep` | Used to search for text or patterns in files or command output |

> 💡 Extra worth knowing: `grep -r "pattern" .` searches recursively through a directory; combine tools with pipes — e.g. `cat access.log | grep "ERROR" | wc -l` — this is where Linux really shines.

---

## Linux Volume Management (LVM)

A storage management system that allows you to create, resize, and combine disk storage dynamically, without worrying about the physical disk layout.

**Key concepts:**
- **Physical Volume (PV)** — the actual physical disk or partition (e.g. `/dev/sdb`).
- **Volume Group (VG)** — a pool created by combining one or more Physical Volumes.
- **Logical Volume (LV)** — a "virtual partition" carved out of a Volume Group; this is what gets formatted and mounted.

**Common commands:**

| Command | Purpose |
|---|---|
| `pvcreate /dev/sdX` | Create a Physical Volume |
| `vgcreate vgname /dev/sdX` | Create a Volume Group from one or more PVs |
| `lvcreate -L 10G -n lvname vgname` | Create a Logical Volume of a given size |
| `lvextend -L +5G /dev/vgname/lvname` | Resize (grow) a Logical Volume |
| `pvdisplay` / `vgdisplay` / `lvdisplay` | View details of PVs / VGs / LVs |

The big advantage: storage can be resized on the fly without unmounting or repartitioning the whole disk — useful for servers where downtime isn't an option.

---

## Vim vs Nano

Both are used to edit the content of a file, but:

- **Nano** — beginner/basic level. Simple, on-screen shortcuts, minimal learning curve.
- **Vim** — built for experienced users. Modal editing (insert/normal/command modes), steeper learning curve, but far more powerful for heavy editing, macros, and scripting.

---

## Contributing

Found an error or want to add something that's missing? PRs and issues are welcome — this is meant to be a living reference that grows as more people learn from it.

## License

Feel free to use, share, and adapt these notes for learning purposes.
