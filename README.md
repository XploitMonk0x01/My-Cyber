# My-Cyber: Practical Cybersecurity Notes and Labs

A curated set of notes, cheatsheets, and hands-on lab snippets for learning system administration, networking, PowerShell, and web exploitation in a safe, controlled environment.

Use this repository for study and lab practice only. Always follow laws, licenses, and organizational policies.

---

## Repository Map

```
MyCyber/
├─ Fundametals/                 # Linux/Windows admin concepts, commands, and security notes
├─ NAS/                         # Networking models, IP addressing, ARP, DHCP, routing, IPv6
├─ Powershell/                  # PowerShell fundamentals guide + resources
├─ SSA/                         # System & Server Administration topics and commands
├─ Web Exploitation Basics/     # SQLi, LFI/RFI, traversal, upload, XSS basics (lab notes)
├─ Payloads/                    # Example payloads (e.g., XSS strings) for lab testing only
├─ network-ports.txt            # Well-known ports quick reference
├─ shell.php                    # Minimal PHP web shell for upload/lab demonstration only
├─ curl command.pdf             # Quick notes on curl usage (PDF)
└─ find command.pdf             # Quick notes on find usage (PDF)
```

---

## What’s Inside

- Fundamentals (Fundametals/)

  - Linux and Windows admin basics: users, permissions, filesystems, processes, logs, compression, networking, and common utilities.
  - Security concepts: CIA triad, authentication/authorization, special permissions (SUID/SGID/Sticky), umask, and more.

- Networking (NAS/)

  - OSI/TCP-IP models, devices, ARP/RARP/ICMP/IGMP, routing basics (RIP/OSPF/BGP/EIGRP).
  - IPv4/IPv6 addressing, subnetting/CIDR, wildcard masks, DHCP concepts and examples.

- System & Server Administration (SSA/)

  - Storage (MBR/GPT/LVM), services, logs, process monitoring (top/htop/ps/lsof), boot flow (GRUB2, initrd), performance, and troubleshooting.
  - Useful commands: modprobe, sysctl, journalctl, systemd tools, swap management, job control, hardware info tools.

- PowerShell (Powershell/)

  - Cmdlets and Verb-Noun conventions, variables, arrays, hashtables, custom objects, pipeline, conditionals, error handling.
  - Includes a quick reference and an external video resource.

- Web Exploitation Basics (Web Exploitation Basics/)

  - Lab-oriented notes on SQL Injection, Directory Traversal, File Inclusion, File Upload, Command Injection, and XSS.
  - References to OWASP resources and sample payloads intended for local, isolated practice environments.

- Payloads

  - XSS payload snippets in `Payloads/xss.txt` to experiment in purpose-built vulnerable apps (e.g., DVWA) only.

- Cheatsheets
  - `network-ports.txt` contains a large list of well-known ports and common services.
  - PDF quick notes for `curl` and `find`.

---

## How to Use This Repo

- Read the sub-folder README files first:

  - Fundamentals: [Fundametals/README.md](Fundametals/README.md)
  - Networking: [NAS/README.md](NAS/README.md)
  - System & Server Admin: [SSA/README.md](SSA/README.md)
  - PowerShell: [Powershell/README.md](Powershell/README.md)
  - Web Exploitation: [Web Exploitation Basics/README.md](Web%20Exploitation%20Basics/README.md)

- Keep experiments isolated:

  - Use disposable VMs/containers and private networks.
  - Never test against systems you do not own or have explicit permission to assess.

- Notes on files in root:
  - `shell.php` is a minimal demonstration file used in upload vulnerability labs. Do not deploy anywhere public.
  - `network-ports.txt` is a handy reference when scanning and enumerating services.

---

## Resources

- PowerShell video: https://youtu.be/Hmkyn4yoLNQ?si=vtd9bupCKuDXlPQK
- OWASP references used in notes:
  - SQL Injection: https://owasp.org/www-community/attacks/SQL_Injection
  - Path Traversal: https://owasp.org/www-community/attacks/Path_Traversal
  - Cross-Site Scripting (XSS): https://owasp.org/www-community/attacks/xss/

---

## Safety, Ethics, and Scope

- Educational use only. Many examples demonstrate insecure configurations and payloads for the purpose of understanding and remediation.
- Perform hands-on work only in controlled lab environments with explicit authorization.
- The authors and contributors are not responsible for misuse.

---

Happy learning and stay safe!
