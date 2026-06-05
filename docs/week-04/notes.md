# Week 4 Notes — Windows OS Security

These classes were all about how Windows works under the hood, and why that matters for security.
Three classes: the first was hands-on demo of Windows internals, the second went deeper into 
security features, and the third was a revision before the quiz.

---

## The Operating System — What It Actually Is

Every device runs on an operating system. Windows, macOS, Android, iOS — they are all operating systems.
The OS is not just a background thing. It is the foundation that everything else runs on top of.

A better way to think about it than the "house" analogy: think of the OS like a **phone's SIM card slot**.
The phone only works because the SIM fits the slot. If you put the wrong SIM in, it does not work.
Every app you install has to be compatible with the OS — that is why you download the Windows version,
not the Mac version, of an app. The OS is what makes compatibility possible.

Windows is the most widely used OS, especially in schools, offices, and enterprises. That popularity
is exactly why attackers target it most. More users means more potential victims.

---

## NTFS — How Windows Organises Your Files

Windows uses a file system called **NTFS**, which stands for New Technology File System.

A file system is basically the rules for how files are stored and organised on your drive.
Think of it like a library's cataloguing system — without it, books (files) would just be piled
anywhere and you could never find anything.

NTFS does three important things:

**Permissions** — it controls who can read, write, or modify a file. When you right-click a file,
go to Properties, then Security, you can see exactly who has what access. You will see entries like
"System", "Administrators", and your own username, each with their own permission level.

**Encryption** — files can be encrypted so only authorised users can open them.

**Auditing** — it can keep a log of who accessed or modified a file and when.
This is extremely useful in cybersecurity — if something goes wrong, you can trace it.

---

## Windows Key Directories

When you open File Explorer, you are looking at directories. These are not just folders — some of
them are special, reserved locations that the operating system depends on.

**Program Files** — this is where your 64-bit applications live. Every app you install creates a
folder here: Chrome, Wireshark, Zoom, all of them. Normal users cannot modify this folder freely.

**Program Files (x86)** — same idea, but for 32-bit applications. The "x86" refers to the older
32-bit processor architecture. If you are downloading software and it asks "32-bit or 64-bit?",
this is why that question exists.

Why does this matter for security? If you ever see a file in either of these directories with a
random name — gibberish, no recognisable words, not associated with any app you installed — that
is a red flag. Malware sometimes hides itself in Program Files because it blends in with legitimate
applications.

**Users** — this is where your personal data lives. Desktop, Documents, Downloads, AppData — all
of it is under your username in the Users folder. AppData is particularly interesting: it stores
your application settings and preferences. That is how your browser remembers your login sessions
and settings every time you reopen it.

**Windows** — this is the most critical directory. It contains the core operating system files,
including the boot files that make your computer start up. Do not delete anything here.
If the boot files get corrupted or deleted, your computer will not turn on. Only OS files
are allowed here — no matter what privilege level an app has, it cannot install itself here.

---

## File Paths — How the Computer Navigates Directories

When you open folders on your screen, the computer is following a path. That path looks like this:

```
C:\Program Files\CorelDraw\Data
```

Reading it left to right: you start at the C: drive, then go into Program Files, then into CorelDraw,
then into Data. Each backslash means "go one level deeper". If you remove "Data" from the end, you
are now pointing at the CorelDraw folder instead.

This matters in cybersecurity because when you are analysing a suspicious file or running a command,
you need to understand where something is located and how to navigate to it.

---

## Task Manager — Your Window Into What Is Running

Task Manager shows you everything that is currently running on your computer.
Open it with **Ctrl + Shift + Esc** (fastest way) or **Ctrl + Alt + Delete** and then click Task Manager.

**Processes tab** — every running application shows up here, including background ones you did not
consciously open. Most will end in .exe.

**Performance tab** — shows CPU usage, RAM usage, disk usage, and network activity in real time.
If your CPU is suddenly at 100% and you are not doing anything heavy, something is using your
computing power without your knowledge.

**Startup Apps tab** — these applications launch automatically every time you start your computer.
Some of them you need (antivirus), some of them you do not (bloatware). Disabling unnecessary
startup apps makes your computer faster and reduces your attack surface.

**How to spot a suspicious process:**
- You do not recognise the name
- The name is gibberish or not in English
- It is using a disproportionate amount of CPU or RAM for no obvious reason
- You try to search for it online and find nothing, or find it associated with malware

If you want to stop a process: right-click it and select **End Task**. If you end a process by
mistake, you just reopen the application.

---

## User Account Control (UAC)

UAC is the pop-up that appears when you try to install software or run something as an administrator.
It asks: "Do you want to allow this app to make changes to your device?"

A clearer way to understand it than the "house" analogy:

Think of UAC like a **bank teller asking for ID**. You can walk into a bank freely, look around, sit
in the waiting area. But the moment you want to access an account or move money, they stop you and
ask to verify who you are. UAC does the same thing — normal tasks work without interruption, but
anything that could change the system requires confirmation.

**Why it matters for security:**
Most malware tries to make changes to the system — installing files, modifying settings, accessing
protected directories. UAC blocks that unless you explicitly say yes. If a UAC prompt appears and
you did not trigger it by doing something yourself, that is a warning sign.

**Elevation of privileges** — this is the term for when an app requests more permissions than a
standard user has. Running something "as administrator" is requesting elevated privileges.

**Separation of roles** — you can run as a standard user (limited permissions) or as an admin
(full permissions). Running as admin all the time is risky because any malware running on your
account would also have admin-level access. Best practice is to run as standard user by default.

---

## Windows Defender

Windows Defender is the built-in antivirus and security tool that comes with Windows. You do not
need to download it — it is already there.

What it does:
- **Real-time protection** — continuously scanning files, downloads, and running processes
- **Threat intelligence** — connected to Microsoft's cloud to recognise new threats as they emerge
- **Firewall and network protection** — monitors network traffic for suspicious activity
- **Device health monitoring** — flags when something is wrong (like memory integrity being off)
- **SmartScreen filter** — warns you before you open a malicious website or download

You can open it by pressing the Windows key and typing "Windows Security". The home screen gives
you a quick overview — green checkmarks mean everything is fine, yellow or red means something
needs your attention. Do not ignore those warnings.

**The quarantine vs removal question** (came up in class): quarantining moves the threat to an
isolated location where it cannot do damage, but does not delete it. Removing deletes it entirely.
If a threat keeps coming back after quarantine, it means something on your system keeps
re-downloading or re-triggering it — the source needs to be found and removed.

---

## Patch Management

This is one of the most important concepts in cybersecurity, and also one of the most overlooked.

**Patch** — a small, specific, often emergency fix. Something was found to be broken or vulnerable
in a piece of software, and instead of waiting for the next major update, the developers push a
patch out immediately to fix just that one thing.

**Update** — a planned, larger release that may include new features, improvements, and multiple
fixes together.

Think of it like this: a patch is fixing a leaking tap the moment it starts dripping. An update
is the scheduled yearly maintenance of the whole plumbing system.

**Why patching matters in cybersecurity:**

When a vulnerability is discovered in software, attackers know about it too. If you have not
patched your system, you are still vulnerable to that known exploit even though a fix exists.
This is how a lot of real-world attacks happen — not zero-days, but known vulnerabilities that
people simply did not patch.

**Types of updates in Windows:**
- Cumulative updates — include all previous fixes bundled together
- Feature updates — add new functionality (like going from Windows 11 22H2 to 23H2)
- Security updates — address specific vulnerabilities without changing anything else
- Driver updates — update the software that controls your hardware (printer, GPU, etc.)

**Automatic vs manual updates:**
For personal computers, turn on automatic updates. You want patches applied as soon as they are
available. For enterprise environments (a company's network of hundreds of machines), admins
prefer manual control — they test updates on non-production machines first to make sure the update
does not break anything before rolling it out everywhere.

Windows 10 reached end of support in October 2025. That means Microsoft will no longer release
patches for it. Running an unsupported OS is a security risk because vulnerabilities discovered
after that date will never be fixed.

---

## Best Practices for Windows Security

These came up across all three classes and are worth having in one place:

**Enable automatic updates** — keep your system patched at all times.

**Use strong passwords** — uppercase, lowercase, numbers, special characters. No personal
information (names, birthdays, phone numbers) that someone who knows you could guess.

**Use MFA (Multi-Factor Authentication)** — even if someone gets your password, they still
cannot get in without the second factor.

**Disable unnecessary services** — anything that is running and you do not need is a potential
entry point. Turn off Remote Desktop if you are not using it.

**Review user accounts** — if multiple people use a computer, each person should have their own
account with appropriate permissions. You, as the owner, should be the admin. Others should
be standard users.

**Regular backups** — back up to at least two locations. Cloud (OneDrive, Google Drive) and an
external drive if possible. If ransomware hits, backups are the only way to recover without paying.

**Be careful with external devices** — flash drives can carry malware. If you must use one, scan
it before opening any files on it. This was a real example from class: a flash drive corrupted
files on a system because it was plugged into an infected machine first.

---

## Things That Actually Came Up in Class (Real Examples)

- If you find a process in Task Manager using as much CPU as Zoom but you do not recognise it,
  that is a red flag for malware.
- A student had a Trojan (Tujan/Trojan) that kept coming back after quarantine — this is because
  the application hosting the malware was not uninstalled, only the detected file was quarantined.
- Antivirus is not foolproof. Free versions often do not cover everything. Rootkits can survive
  a format because they embed themselves at the OS level.
- A flash drive can carry malware from one machine to another. Corrupted image files that only
  load partially are a sign the virus attacked the file itself.
- Windows 10 end of support: October 2025. No more patches after that date.

---

## Quick Reference

| Term | What it means |
|---|---|
| NTFS | Windows file system — handles permissions, encryption, auditing |
| UAC | Pop-up that asks permission before system changes |
| Windows Defender | Built-in antivirus — no download needed |
| Patch | Emergency small fix for a specific vulnerability |
| Update | Planned larger release with new features and fixes |
| Elevation of privileges | Requesting admin-level access |
| Quarantine | Isolating a threat so it cannot spread |
| AppData | Where your application settings and session data are stored |
| End Task | Stopping a running process in Task Manager |
