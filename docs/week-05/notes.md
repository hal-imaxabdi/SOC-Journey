# Week 5 Notes — Linux Operating System Security

Three classes this week: the first two were hands-on demos with the instructor and a student
sharing their screen, and the third was a revision class by Tutor Jeremiah who also did a live
demo of SSH remote access. A lot happened this week, and it was one of the most practical weeks
so far.

---

## Why Linux Matters in Cybersecurity

Before jumping into commands, it helps to understand why this week exists.

Most cybersecurity tools run on Linux. Wazuh, Burp Suite, Nmap, Wireshark, OWASP ZAP — they all
work on Linux. Most servers on the internet run Linux. When you work in a SOC, you will deal with
Linux systems constantly, whether you are running tools, reading logs, or investigating a
compromised server.

If you do not know how to navigate Linux, you cannot use the tools. It is that simple.

---

## What Linux Actually Is

Linux is an open source operating system. Open source means that the source code is publicly
available — anyone can read it, modify it, and build their own version of it.

That is why there are so many "flavors" of Linux, which are officially called **distros**
(short for distributions). Think of it like this: the base Linux is like raw rice. Ubuntu,
Kali, Parrot, Fedora, Mint — these are all different dishes made from that same rice, each
with different additions and purposes.

- **Kali Linux** — built for cybersecurity. Comes preloaded with security tools.
- **Ubuntu** — popular for general use and servers. Beginner friendly.
- **Parrot OS** — lightweight, also built for security professionals.
- **Fedora / CentOS / Red Hat** — common in enterprise server environments.

Most of the class uses Kali, running inside a virtual machine on Windows. That is the
recommended setup — running it in a VM means if you break something, you can just reset it
without touching your actual computer.

---

## Navigating the Linux Terminal

Everything in Linux can be done through the terminal. You do not always need to click around
in a GUI. In fact, most cybersecurity work happens in the terminal.

Here are the commands covered across all three classes:

### Listing and Moving Around

```
ls                        List files and folders in current directory
ls -l                     List files with their permissions
ls -l filename            Check permissions for a specific file
cd Desktop                Move into the Desktop directory
cd ..                     Go back one directory
cd ../..                  Go back two directories
```

Linux is **case sensitive**. `Desktop` and `desktop` are different things. Type it exactly
as it appears.

The tilde symbol `~` means your home directory. When you see `~/Desktop`, it means you are
inside the Desktop folder inside your home directory.

### Creating and Editing Files

```
touch filename.txt        Create a new empty file
nano filename.txt         Open a file in the Nano text editor
cat filename.txt          Print the contents of a file to the terminal
echo "text" > file.txt    Write text into a file (overwrites existing content)
echo "text" >> file.txt   Add text to a file without overwriting
```

When you are inside Nano:
- **Ctrl + S** to save
- **Ctrl + X** to exit

### Creating and Deleting

```
mkdir foldername          Create a new directory (folder)
rmdir foldername          Delete an empty directory
rm filename.txt           Delete a file
cp file1.txt file2.txt    Copy a file (creates a duplicate with a new name)
mv oldname.txt newname.txt  Rename a file
mv file.txt /path/        Move a file to a different directory
```

### Other Useful Commands

```
whoami                    Shows who the current logged-in user is
clear                     Clears the terminal screen
history                   Shows a list of all previously used commands
```

**Tab completion** — when typing a filename or command, press the Tab key and Linux will
autocomplete it for you. This saves time and reduces typos.

**Arrow up key** — press the up arrow to scroll through your previous commands instead of
retyping them.

---

## File Permissions — How Linux Controls Access

This was the main topic of the week, and it is one of the most important concepts in Linux
security.

Every file and directory in Linux has permissions that control three things:
- Who can **read** it (view the contents)
- Who can **write** to it (modify or delete)
- Who can **execute** it (run it as a program or script)

And those permissions are set for three groups:
- **Owner** — the person who created the file, or the current user
- **Group** — a set of users who share the same role or access level
- **Others** — everyone else who is not the owner and not in the group

### Reading the Permission String

When you run `ls -l`, you see something like this:

```
-rw-rw-r-- 1 kali kali 0 Apr 14 file.txt
```

Break it down like this:

```
-  rw-  rw-  r--
|  |    |    |
|  |    |    └── Others: read only
|  |    └─────── Group: read and write
|  └──────────── Owner: read and write
└─────────────── File type (dash = file, d = directory, l = symbolic link/shortcut)
```

The letters mean:
- `r` = read
- `w` = write
- `x` = execute
- `-` = no permission for that slot

**Important:** The first character before the 9 permission characters tells you what you
are looking at. `d` means directory, `-` means file, `l` means symbolic link (a shortcut
that points to another file or folder).

### The Number System (Octal Permissions)

Each permission has a fixed value:
- Read = **4**
- Write = **2**
- Execute = **1**

To get the permission number for a group, add up whatever permissions they have:
- Read + Write + Execute = 4 + 2 + 1 = **7** (full access)
- Read + Write = 4 + 2 = **6**
- Read only = **4**
- No permissions = **0**

You write this as three digits, one for each group (Owner, Group, Others):

| Permission | Meaning |
|---|---|
| 777 | Everyone has full access — very risky, not recommended |
| 755 | Owner has full access, group and others can read and execute |
| 644 | Owner can read and write, group and others can only read |
| 600 | Only owner can read and write, no one else has any access |
| 400 | Owner can only read, no one else has any access |

### Changing Permissions — chmod

`chmod` stands for "change mode". It changes the permissions on a file.

```
chmod 600 filename.txt    Only owner can read and write
chmod 777 filename.txt    Everyone has full access (avoid this)
chmod 755 filename.txt    Standard for scripts/executables
chmod 400 filename.txt    Read only for owner
```

### Changing Ownership — chown

`chown` stands for "change owner". It changes who owns a file.

```
chown newuser:newgroup filename.txt
```

In the demo, the instructor changed ownership of a file from `kali` to a new user called
`joanne`. After doing that, when logged in as `kali`, the file could only be read — not
written to — because `kali` was now under "others" for that file, and others only had
read permission.

This is exactly how permission errors happen in real life. You are the wrong user, or
the file has the wrong owner, and access gets blocked.

---

## Users and Groups

You can create and manage users directly from the terminal.

```
sudo useradd username       Create a new user
sudo passwd username        Set a password for a user
sudo usermod                Modify a user's settings or permissions
sudo userdel username       Delete a user
su username                 Switch to a different user (su = switch user)
whoami                      Confirm which user you are currently logged in as
```

`sudo` is the Linux equivalent of "Run as Administrator" in Windows. It temporarily borrows
admin (root) privileges to run a specific command. Use it only when needed — not for
everything.

`su` switches you to a different user entirely. If you want to switch to root (the admin
account), you use `sudo su`.

In the demo, a new user called `joanne` was created. The instructor then changed a file's
ownership to `joanne`. When still logged in as `kali`, writing to that file was denied,
because `kali` was now just an "other" user with read-only access. This demonstrated
exactly how file ownership and permissions interact.

---

## Package Management and Updates

Linux manages software using a **package manager** — a tool that handles installing,
updating, and removing software. Think of it like an app store, but from the terminal.

The package manager depends on which distro you are using:

| Distro | Package Manager | Commands |
|---|---|---|
| Kali / Ubuntu (Debian-based) | APT | `apt install`, `apt update`, `apt upgrade` |
| Fedora / CentOS (Red Hat-based) | YUM or DNF | `yum install`, `yum update` |
| Arch Linux | Pacman | `pacman install`, `pacman update` |

Since most of the class uses Kali, APT is what matters here.

```
sudo apt update                          Refresh the list of available updates
sudo apt upgrade                         Install all available updates
sudo apt install nmap                    Install a specific package
sudo apt remove --purge telnet           Remove a package completely
apt list --upgradeable                   See which packages have updates available
```

**Why does this matter for security?**

Outdated software has known vulnerabilities. Attackers know about those vulnerabilities too.
If you do not update, you are vulnerable to exploits that already have patches available.
This is not a hypothetical — most real-world attacks exploit known, patchable vulnerabilities
rather than new zero-days.

Telnet came up as a specific example. It is an old, insecure protocol that sends everything
in plain text — including passwords. It should not be installed. If it is on your system,
remove it.

---

## Hardening Linux

Hardening means securing your system by reducing the attack surface — closing doors you do
not need open.

The analogy from class: if you have a back door in your house that nobody ever uses, lock it.
Do not leave it open just because you are not using it. An attacker who has been watching your
house knows it is open, and that is how they will get in.

**Key hardening steps covered:**

**1. Disable unnecessary services**

Services that are running but not needed are potential entry points. Use these to manage them:

```
sudo systemctl status ssh           Check if SSH is running
sudo systemctl stop ssh             Stop the SSH service
sudo systemctl disable ssh          Disable SSH so it does not start on boot
sudo systemctl start ssh            Start it again when needed
sudo systemctl enable ssh           Enable it to start on boot again
```

**2. Configure the firewall (UFW)**

UFW stands for Uncomplicated Firewall. It is built into Kali and Ubuntu.

```
sudo ufw status             Check if the firewall is on or off
sudo ufw enable             Turn the firewall on
sudo ufw disable            Turn it off
sudo ufw allow 22           Allow traffic on port 22 (SSH)
sudo ufw deny 22            Block traffic on port 22
```

**3. Remove insecure packages**

Remove anything you are not using, especially old and insecure services like Telnet:
```
sudo apt remove --purge telnet
```

**Warning from class:** Do not harden your system so aggressively that you lock yourself out.
It has happened. Balance security with usability.

---

## SSH and Remote Access (Tutor Jeremiah's Demo)

This was the most hands-on part of the week, and it was a live demonstration of why
hardening matters.

**SSH** stands for Secure Shell. It is a protocol that lets you connect to another computer
remotely and control it through a terminal. It runs on **port 22**.

In the demo, Jeremiah used a Windows tool called **PuTTY** to connect to his Kali Linux
virtual machine over SSH. Once connected, he was able to:
- List files on the Linux machine from Windows
- Create new files on the Linux machine
- Edit files on the Linux machine
- Delete files on the Linux machine
- Install software on the Linux machine

All of this from a completely separate system.

This is not magic — this is what SSH is designed to do. But it is also exactly what an
attacker would do if they got SSH access to a server they should not have.

**How to prevent this (hardening in practice):**

After the demo, Jeremiah showed how to lock it down:
```
sudo ufw deny 22             Block the SSH port in the firewall
sudo systemctl stop ssh      Stop the SSH service
sudo systemctl disable ssh   Prevent it from starting again
```

After doing this, trying to connect with PuTTY gave a "Connection refused" error. The
door was closed.

This is what hardening looks like in practice — not theory, but actually turning off
what you do not need.

---

## AuditD — Monitoring File Access

Came up briefly in the third class. AuditD is a tool that logs security-relevant events
on your Linux system — including who accessed files and who made changes to them.

Even if an attacker deletes logs, AuditD records the fact that logs were deleted. You
cannot fully erase your trail if auditing is properly configured.

This connects directly to what you will do as a SOC analyst — reading audit logs to
reconstruct what happened on a compromised system.

---

## Quick Reference

| Command | What it does |
|---|---|
| `ls` | List files in current directory |
| `ls -l` | List files with permissions |
| `cd foldername` | Move into a folder |
| `cd ..` | Go back one level |
| `touch file.txt` | Create a new empty file |
| `nano file.txt` | Open file in text editor |
| `cat file.txt` | Print file contents |
| `mkdir foldername` | Create a new directory |
| `rm file.txt` | Delete a file |
| `chmod 600 file.txt` | Change file permissions |
| `chown user:group file.txt` | Change file ownership |
| `whoami` | Show current user |
| `sudo` | Run a command as admin |
| `su username` | Switch to a different user |
| `sudo apt update` | Refresh package list |
| `sudo apt upgrade` | Install updates |
| `sudo ufw status` | Check firewall status |
| `sudo ufw enable` | Turn on firewall |
| `sudo systemctl status ssh` | Check if SSH is running |
| `sudo systemctl stop ssh` | Stop SSH service |
| `history` | Show previous commands |
| `clear` | Clear the terminal screen |
