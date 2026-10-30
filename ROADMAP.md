# OSCP Study Roadmap

> Compiled from 30+ real pass/fail reports, community guides, and OffSec resources.
> Tailored for: someone with web security + mobile app security background, some networking, zero AD/privesc/tunneling.

---

## 1. The Exam at a Glance

| Detail | Value |
|---|---|
| **Full name** | PEN-200 / OffSec Certified Professional |
| **Format** | 100% hands-on practical (no multiple choice) |
| **Duration** | 23 hours 45 minutes hacking + 24 hours for report |
| **Machines** | 3 standalone (20 pts each) + 1 Active Directory set (40 pts) |
| **Total points** | 100 |
| **Pass score** | 70 points |
| **Validity** | OSCP (lifetime) + OSCP+ (3 years, renewable) |
| **Cost** | ~$1,749 USD (course + 90 days lab + 1 exam attempt) |
| **Restrictions** | No sqlmap, no commercial tools, Metasploit on 1 machine only |

**Critical insight from passers**: You cannot pass by skipping Active Directory. The AD set is 40 points. Even if you root all 3 standalones perfectly (60 pts), you still need 10 from AD. Most passers say: **get the full AD set (40 pts) + 2 standalones (40 pts) = 80 pts** as the target strategy.

---

## 2. Skills You Need (Mapped to Your Background)

### What you already have (leverage these)
- **Web application attacks** - SQLi, XSS, file inclusion, command injection (this IS on the exam)
- **Networking basics** - TCP/IP, ports, protocols (build on this)
- **Security mindset** - you understand attacker thinking from mobile/web work

### What you need to build from zero

| Skill Area | Exam Weight | Your Gap | Priority |
|---|---|---|---|
| **Active Directory attacks** | ~40% of exam | Zero | CRITICAL |
| **Linux privilege escalation** | High (standalones) | Zero | CRITICAL |
| **Windows privilege escalation** | High (standalones + AD) | Zero | CRITICAL |
| **Enumeration methodology** | Foundational to everything | Partial | CRITICAL |
| **Tunneling & pivoting** | Required for AD chain | Zero | HIGH |
| **Password attacks** | Common on exam | Low | HIGH |
| **Public exploit finding & modification** | Common on exam | Low | HIGH |
| **Bash/Python scripting** | Supporting skill | Partial | MEDIUM |
| **Report writing** | Required to pass | Zero | MEDIUM |
| **Buffer overflow** | Reduced in current exam | Zero | LOW |

---

## 3. The Roadmap (5 Phases, ~4-6 Months)

### Phase 0: Foundations (Weeks 1-3)
> Goal: Solid Linux, networking, and tooling base. You can't hack what you can't navigate.

**What to learn:**
- Linux command line fluency (file system, permissions, processes, services, cron)
- Networking deep dive: TCP/IP, DNS, SMB, SNMP, HTTP, FTP protocols
- Bash scripting basics (loops, conditionals, parsing output)
- Python scripting basics (requests, sockets, modifying exploits)
- Set up your attack VM (Kali Linux)

**TryHackMe rooms:**
- [Pre Security](https://tryhackme.com/path/outline/presecurity) path (if networking is shaky)
- [Linux Fundamentals Part 1-3](https://tryhackme.com/room/linuxfundamentalspart1)
- [Bash Scripting](https://tryhackme.com/room/bashscripting)
- [Intro to Networking](https://tryhackme.com/room/introtonetworking)
- [Network Services 1 & 2](https://tryhackme.com/room/networkservices)
- [Python Basics](https://tryhackme.com/room/pythonbasics)

**Milestone:** You can SSH into a box, navigate the filesystem, write a short Bash loop, and explain what happens in a 3-way TCP handshake.

---

### Phase 1: Core Pentesting Skills (Weeks 4-8)
> Goal: Learn the attack lifecycle - enumerate, exploit, escalate. Build your methodology.

**What to learn:**
- **Enumeration** (~40% of your exam time lives here)
  - Port scanning with Nmap (SYN scan, version detection, scripts)
  - Web enumeration: Gobuster/Feroxbuster, Nikto
  - SMB enumeration: enum4linux, smbclient, NetExec
  - SNMP enumeration: snmpwalk, onesixtyone
- **Web attacks** (build on your existing knowledge)
  - SQL injection (manual - sqlmap is banned on exam)
  - Local/Remote File Inclusion (LFI/RFI)
  - Command injection
  - File upload vulnerabilities
  - Directory traversal
- **Finding & modifying public exploits**
  - SearchSploit / ExploitDB
  - Reading and adapting exploit code (don't just run blindly)
- **Shells & payloads**
  - Reverse shells, bind shells, web shells
  - Msfvenom payload generation
  - Shell stabilization

**TryHackMe rooms:**
- [Jr Penetration Tester](https://tryhackme.com/path/outline/jrpenetrationtester) path (89 rooms, rebuilt for 2026 - this is your core path)
- [Nmap](https://tryhackme.com/room/furthernmap)
- [OWASP Top 10](https://tryhackme.com/room/owasptop10)
- [SQL Injection](https://tryhackme.com/room/sqlinjectionlm)
- [Upload Vulnerabilities](https://tryhackme.com/room/uploadvulns)
- [What the Shell](https://tryhackme.com/room/introtoshells)
- [Metasploit Introduction](https://tryhackme.com/room/metasploitintro)

**Also do:**
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) (free) - SQLi and file inclusion labs especially

**Build your methodology document now.** Every passer who wrote about their experience stressed having a repeatable checklist. Start yours:
```
1. Nmap full port scan
2. Service version scan on open ports
3. For each service: enumerate (HTTP -> gobuster; SMB -> enum4linux; etc.)
4. Check for known vulns (searchsploit, google "service version exploit")
5. Try default creds
6. Look for sensitive files/info disclosure
```

**Milestone:** You can enumerate a target, find a web vulnerability, get a shell, and explain each step.

---

### Phase 2: Privilege Escalation (Weeks 9-13)
> Goal: You got a shell. Now become root/SYSTEM. This is where most beginners get stuck.

**Linux Privilege Escalation - learn these techniques:**
- SUID/SGID binaries (check GTFOBins)
- Cron job abuse
- Writable PATH hijacking
- Kernel exploits (last resort)
- Sudo misconfigurations (`sudo -l` is your best friend)
- Capabilities
- NFS no_root_squash
- Sensitive file discovery (passwords in config files, SSH keys, history files)

**Windows Privilege Escalation - learn these techniques:**
- Service misconfigurations (unquoted service paths, weak permissions)
- Always Install Elevated
- Scheduled tasks
- Token impersonation (SeImpersonatePrivilege -> Potato attacks)
- Registry autoruns
- DLL hijacking
- Credential harvesting (SAM, LSASS, cached creds)
- UAC bypass

**Tools to master:**
- linPEAS / winPEAS (automated enumeration)
- pspy (catch cron jobs and running processes on Linux)
- PowerUp (Windows privesc)
- Seatbelt (Windows enumeration)
- GTFOBins (Linux) / LOLBAS (Windows)

**TryHackMe rooms:**
- [Linux Privilege Escalation](https://tryhackme.com/room/linprivesc)
- [Windows Privilege Escalation](https://tryhackme.com/room/windowsprivesc20)
- [Linux PrivEsc Arena](https://tryhackme.com/room/dvwa) (or similar guided rooms)
- [Windows PrivEsc Arena](https://tryhackme.com/room/windowsprivescarena)
- [Sudo Security Bypass](https://tryhackme.com/room/dvwa)
- [Steel Mountain](https://tryhackme.com/room/steelmountain) (Windows box, great practice)
- [Alfred](https://tryhackme.com/room/alfred) (Windows box)
- [Kenobi](https://tryhackme.com/room/kenobi) (Linux box with SMB + privesc)

**Milestone:** Given a low-priv shell on Linux or Windows, you can systematically find and exploit a privesc path within 30-60 minutes.

---

### Phase 3: Active Directory (Weeks 14-18)
> Goal: This is the make-or-break section. 40% of the exam. Learn it cold.

**The AD attack chain you need to know:**

```
Initial Access (phishing, web app on domain machine)
    -> Enumerate the domain (BloodHound, PowerView, ldapsearch)
    -> Find attack paths (Kerberoastable accounts, AS-REP roastable users, 
       delegations, group memberships)
    -> Credential attacks (Kerberoasting, AS-REP roasting, password spraying)
    -> Crack hashes offline (hashcat, john)
    -> Lateral movement (Pass-the-Hash, psexec, wmiexec, evil-winrm)
    -> Escalate in domain (DCSync, Golden/Silver tickets)
    -> Compromise Domain Controller
```

**Specific techniques to master:**
1. **Enumeration**: BloodHound + SharpHound collector, PowerView, ldapdomaindump, NetExec
2. **Kerberoasting**: GetUserSPNs.py (Impacket) or Rubeus - targets service accounts
3. **AS-REP Roasting**: GetNPUsers.py - targets accounts with pre-auth disabled
4. **Pass-the-Hash**: psexec.py, wmiexec.py, evil-winrm with hash
5. **Pass-the-Ticket**: export and reuse Kerberos tickets
6. **DCSync**: secretsdump.py - pull all domain hashes
7. **Password spraying**: NetExec against SMB/LDAP
8. **Token impersonation**: on compromised machines

**Tools - the Impacket suite is essential:**
- GetUserSPNs.py, GetNPUsers.py, secretsdump.py
- psexec.py, wmiexec.py, smbexec.py
- BloodHound + SharpHound / BloodHound.py
- NetExec (successor to CrackMapExec)
- evil-winrm
- Rubeus, Mimikatz (on Windows targets)

**TryHackMe rooms:**
- [Active Directory Basics](https://tryhackme.com/room/winadbasics)
- [Attacktive Directory](https://tryhackme.com/room/attacktivedirectory)
- [Attacking Kerberos](https://tryhackme.com/room/attackingkerberos)
- [Post-Exploitation Basics](https://tryhackme.com/room/postexploit)
- [Hacking Active Directory](https://tryhackme.com/room/dvwa) (search for AD-focused rooms)
- [Wreath](https://tryhackme.com/room/wreath) (multi-machine network with pivoting)

**HackTheBox (retired, need VIP):**
- **Forest** - AS-REP roasting -> BloodHound -> DCSync (excellent starter AD box)
- **Active** - Kerberoasting classic
- **Sauna** - AS-REP roasting + WinRM + DCSync
- **Monteverde** - Azure AD enumeration
- **Resolute** - password spraying + DnsAdmins group abuse
- **Cascade** - LDAP enumeration + credential recovery

**Build your own lab (optional but recommended):**
- 1 Windows Server (Domain Controller) + 1-2 Windows 10 workstations
- Practice the full chain: enumerate -> roast -> move -> own DC
- Free guide: "Building an AD Lab" by TheMayor on YouTube

**Milestone:** You can compromise a 3-machine AD set (DC + 2 workstations) end-to-end in under 4 hours.

---

### Phase 4: Tunneling, Pivoting & Exam Prep (Weeks 19-24)
> Goal: Tie it all together. Practice under exam conditions.

**Tunneling & Pivoting (required for AD chains):**
- SSH local/remote/dynamic port forwarding
- Chisel (HTTP tunneling - one passer said they failed because they didn't know this tool)
- Ligolo-ng (modern alternative)
- sshuttle
- Proxychains configuration
- socat port forwarding

**Password Attacks:**
- Hashcat and John the Ripper (know the hash types: NTLM, Kerberos, bcrypt, MD5, SHA)
- Hydra for online brute force
- Wordlist management (rockyou, SecLists, custom lists with rules)

**Report Writing:**
- Use the official OffSec report template
- Document as you go (screenshots + commands + output)
- Tools: Obsidian, CherryTree, or Notion for notes
- Every machine needs: enumeration steps, exploitation proof, escalation proof, screenshots of flags

**Practice Machines - The Grind:**

This is where most of your time should go. Target **40-70 machines minimum** before the exam.

| Platform | What to do | Notes |
|---|---|---|
| **Proving Grounds Practice** | Work through LainKusanagi's list | Closest to exam. Made by OffSec. Do these first. |
| **Proving Grounds Play** | Warm-up machines | Free tier, easier boxes |
| **HackTheBox** | TJ Null's OSCP-like list (retired boxes) | Need VIP subscription. Harder than exam sometimes. |
| **TryHackMe** | Offensive Pentesting path boxes | More guided, good for building confidence |
| **PEN-200 Labs** | Course challenge labs | Do ALL of these when you buy the course |

**Practice order:**
1. Start with easy Proving Grounds Play boxes
2. Move to Proving Grounds Practice (LainKusanagi list, sorted by difficulty)
3. Mix in HackTheBox TJ Null list boxes
4. Do PEN-200 course labs
5. Mock exams in the final 2-3 weeks

**Mock Exam Strategy (from passers):**
- Pick 3 standalone-difficulty machines + 1 AD set from Proving Grounds
- Set a 24-hour timer (or split into 24 one-hour blocks over several days)
- No hints, no write-ups
- Write a full report after
- Do this at least twice before the real exam

---

## 4. Lessons From People Who Failed (Then Passed)

These patterns appear across dozens of fail-then-pass stories:

### The #1 killer: Rabbit holes
> "I wasted 7.5 hours trying to pivot laterally when I should have moved on after 1 hour."

**Rule: If you're stuck for 60-90 minutes with no progress, move to another machine.** Come back later with fresh eyes.

### Unbalanced practice
> "~80% of the boxes I'd solved before my first attempt were Linux."

The exam has Windows standalones AND an AD set. Practice both OS types equally. Many beginners gravitate toward Linux because it's comfortable - force yourself onto Windows boxes.

### Weak enumeration
> "I missed obvious low-hanging fruit because I rushed through enumeration."

Enumeration is not the boring part before the fun part. Enumeration IS the exam. Build a checklist and follow it religiously on every machine.

### No mock exam
> "Built mock exams - this helped with mental conditioning and stress more than anything."

The 24-hour format is brutal. If your first time doing it is the real exam, you will struggle with fatigue and time management.

### Skipping the report
> "The report is half the exam."

Many people lose points not because they couldn't hack the machine, but because their documentation was incomplete. Screenshot everything. Document commands. Write the report AS you hack, not after.

### Starting with the wrong machine
> "I tackled the Active Directory set first because it looked easy and got stuck."

Popular exam-day ordering from passers:
1. Start with the easiest-looking standalone (build momentum, ~1-2 hours)
2. Move to the AD set while still fresh (~3-4 hours)
3. Tackle remaining standalones
4. Return to anything you skipped

### Burnout
> "I didn't realize how mentally burnt out I was."

This is a marathon, not a sprint. Take rest days. Study 2-3 hours on weekdays, 4-6 on weekends. Don't cram 12-hour days for months.

---

## 5. Tools Cheat Sheet

Every passer mentions these tools. Know them cold before exam day.

### Enumeration
| Tool | Purpose |
|---|---|
| Nmap | Port scanning, service detection, NSE scripts |
| Gobuster / Feroxbuster | Directory and file brute-forcing |
| Nikto | Web server scanner |
| enum4linux / enum4linux-ng | SMB/NetBIOS enumeration |
| NetExec (nxc) | Swiss army knife for SMB, LDAP, WinRM, MSSQL |
| snmpwalk | SNMP enumeration |
| ldapsearch | LDAP enumeration |
| BloodHound + SharpHound | AD attack path visualization |

### Exploitation
| Tool | Purpose |
|---|---|
| SearchSploit | Offline ExploitDB search |
| Metasploit | Framework (limited to 1 machine on exam) |
| Msfvenom | Payload generation |
| Impacket suite | AD attacks, remote execution |
| evil-winrm | WinRM shell access |
| Burp Suite (Community) | Web app testing |

### Privilege Escalation
| Tool | Purpose |
|---|---|
| linPEAS / winPEAS | Automated privesc enumeration |
| pspy | Linux process monitoring (find cron jobs) |
| PowerUp | Windows privesc checks |
| GTFOBins | Linux binary exploitation reference |
| LOLBAS | Windows living-off-the-land binaries |

### Tunneling
| Tool | Purpose |
|---|---|
| Chisel | HTTP tunneling (LEARN THIS) |
| Ligolo-ng | Tunneling through pivots |
| SSH | Local/remote/dynamic port forwarding |
| Proxychains | Route tools through SOCKS proxy |
| sshuttle | VPN-like SSH tunneling |

### Password Cracking
| Tool | Purpose |
|---|---|
| Hashcat | GPU-accelerated hash cracking |
| John the Ripper | CPU-based hash cracking |
| Hydra | Online brute force |

---

## 6. Exam Day Strategy (From Passers)

### Before the exam
- [ ] Sleep well the night before (seriously - every passer says this)
- [ ] Prepare food/drinks in advance (you won't want to cook during)
- [ ] Test VPN connection and exam environment
- [ ] Have your methodology notes and cheat sheets ready
- [ ] Template your report (OffSec template pre-filled with sections)

### During the exam (23h 45m)

| Time Block | Activity |
|---|---|
| **Hours 0-2** | Read all machines. Start with the easiest-looking standalone. |
| **Hours 2-6** | Move to the AD set while fresh. Target full compromise. |
| **Hours 6-10** | Tackle the remaining standalones. |
| **Hours 10-12** | Take a real break. Sleep if needed. Eat. |
| **Hours 12-18** | Return to anything unfinished with fresh eyes. |
| **Hours 18-24** | Polish documentation. Screenshot review. |

### Rules from passers
- **Timebox every machine**: 2-3 hours max per standalone, 4 hours for AD. Move on if stuck.
- **Screenshot EVERYTHING**: IP visible, hostname, user flag, proof.txt
- **Document commands as you run them** - copy/paste into notes immediately
- **Take breaks every 2-3 hours** for 10-15 minutes
- **Don't panic if stuck** - every passer describes getting stuck at some point
- **Run scans in the background** while working on another machine

### After the exam (24h for report)
- Compile all screenshots and notes
- Use OffSec's report template
- Include: enumeration steps, vulnerability found, exploitation proof, privesc method, flags
- Proofread everything
- Submit well before the deadline

---

## 7. Recommended Study Schedule

For someone studying part-time (2-3 hours weekdays, 4-6 hours weekends):

| Week | Phase | Focus |
|---|---|---|
| 1-3 | **Foundations** | Linux, networking, scripting, Kali setup |
| 4-5 | **Enumeration** | Nmap, web enum, SMB, methodology building |
| 6-8 | **Web attacks + shells** | SQLi, LFI/RFI, command injection, reverse shells |
| 9-11 | **Linux privesc** | SUID, cron, sudo, kernel exploits + practice boxes |
| 12-13 | **Windows privesc** | Services, tokens, registry, potato attacks + practice boxes |
| 14-16 | **Active Directory** | Enumeration, Kerberoasting, lateral movement |
| 17-18 | **AD continued** | Full AD chains, BloodHound, DCSync |
| 19-20 | **Tunneling & pivoting** | SSH tunneling, Chisel, Ligolo, proxychains |
| 21-22 | **Machine grinding** | Proving Grounds + HTB machines from lists (target 40-70 total) |
| 23-24 | **Mock exams + review** | 2 full mock exams, report practice, weak area review |

**Buy PEN-200 around Week 14-16.** You'll get the most out of the 90-day lab access if you already have foundations. Course material reinforces what you've learned, and lab machines become actual practice instead of confusion.

---

## 8. Free & Paid Resources

### Free
- [TryHackMe](https://tryhackme.com) - Many free rooms, structured paths
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) - Best free web app hacking labs
- [HackTricks](https://book.hacktricks.wiki) - Encyclopedia of techniques
- [GTFOBins](https://gtfobins.github.io) - Linux binary privesc reference
- [LOLBAS](https://lolbas-project.github.io) - Windows binary reference
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) - Payload reference
- [CyberChef](https://gchq.github.io/CyberChef/) - Encoding/decoding/crypto
- [IppSec YouTube](https://www.youtube.com/@ippsec) - HackTheBox walkthroughs (gold mine)

### Paid (worth it)
- **PEN-200 Course** (~$1,749) - Required for exam eligibility
- **Proving Grounds Practice** (~$19/month) - Closest to exam
- **HackTheBox VIP** (~$14/month) - Access retired machines
- **TryHackMe Premium** (~$14/month) - All rooms unlocked

---

## 9. Tracking Your Progress

Use this repo to track everything:

```
OSCP/
  ROADMAP.md              <- You are here
  notes/
    methodology.md        <- Your personal attack methodology
    linux-privesc.md      <- Linux privesc notes
    windows-privesc.md    <- Windows privesc notes  
    active-directory.md   <- AD attack chain notes
    tunneling.md          <- Tunneling/pivoting notes
    web-attacks.md        <- Web attack notes
    tools-cheatsheet.md   <- Quick reference commands
  machines/
    README.md             <- Machine tracking log
    <machine-name>.md     <- Write-up per machine you root
  reports/
    mock-exam-1.md        <- Mock exam report
    mock-exam-2.md        <- Mock exam report
```

Share your progress with me as you go. I'll help you:
- Understand concepts you're stuck on
- Review your methodology
- Suggest next machines to practice on
- Quiz you on techniques
- Help with report writing
- Debug your approach when you're stuck on a box

---

## 10. Your Immediate Next Steps

1. **This week**: Set up Kali Linux VM. Start TryHackMe Pre Security path if networking is shaky, or jump straight to Linux Fundamentals.
2. **Sign up**: TryHackMe (free to start), create accounts on HackTheBox and Proving Grounds.
3. **Start a methodology document**: Even blank, create the file. You'll fill it as you learn.
4. **Tell me where you are**: After your first week, share what you've done and what confused you. We'll adjust the plan.

---

*Sources used to build this roadmap listed at the bottom of the accompanying summary.*
