# IKB21403 Vulnerability Analysis  

# Table of Contents

1. Introduction  
2. Lab Setup  
3. Challenge 1 – Fast Nmap Scan  
4. Challenge 2 – TTL OS Fingerprinting  
5. Challenge 3 – FTP Banner Enumeration  
6. Challenge 4 – Anonymous FTP Login  
7. Challenge 5 – Version Detection  
8. Challenge 6 – OS Detection  
9. Challenge 7 – Enum4linux  
10. Challenge 8 – NFS Exports  
11. Challenge 9 – RPC Info  
12. Challenge 10 – SNMPWalk  
13. Final Conclusion  

---

# 1. Introduction

Enumeration is the process of gathering useful technical information from a target system. This phase is important in vulnerability analysis because it helps identify open ports, active services, operating systems, usernames, shared folders, and weak configurations.

This report demonstrates ten enumeration challenges using a virtual lab environment consisting of one attacker machine and one victim machine.

---

# 2. Lab Setup

## Attacker Machine
- Kali Linux

## Victim Machine
- Metasploitable 2

## Virtualization Software
- VirtualBox / VMware

## Network Configuration
- NAT Network

## Victim IP Address
```bash
10.0.2.15
```

---


3. Challenge 1 – Fast Nmap Scan
Purpose

To quickly detect common open ports on the target machine.
<img width="527" height="159" alt="Screenshot 2026-04-29 164251" src="https://github.com/user-attachments/assets/c2ccefdd-3bd0-406c-b479-9cb450ca2ca6" />
Command
nmap -F 10.0.2.15


Explanation

The -F option performs a fast scan using common ports only. This reduces scan time while still identifying important services.

Findings
Port 21 FTP
Port 22 SSH
Port 23 Telnet
Port 80 HTTP
Reason

Open ports indicate services available to network users.

Conclusion

Several services were exposed on the target machine.

---

4. Challenge 2 – TTL OS Fingerprinting
Purpose

To estimate the target operating system based on TTL value.

Command
ping 10.0.2.15
<img width="912" height="404" alt="Screenshot 2026-04-29 164402" src="https://github.com/user-attachments/assets/d779a8be-7a03-4b91-a607-c0cbbcd1e9ee" />


Explanation

TTL (Time To Live) values can indicate likely operating systems.

TTL 64 = Linux
TTL 128 = Windows
Findings

TTL = 64

Reason

Linux systems commonly use TTL 64.

Conclusion

The victim machine is likely Linux-based.

---

5. Challenge 3 – FTP Banner Enumeration
Purpose

To identify the FTP server software and version.

Command
nc 10.0.2.15 21
<img width="415" height="59" alt="Screenshot 2026-04-29 164447" src="https://github.com/user-attachments/assets/c66009b6-2a2a-486c-8b7f-399741a0d639" />


Explanation

Banner grabbing connects to the service and reads the welcome message.

Findings
220 (vsFTPd 2.3.4)
Reason

Service versions help identify outdated software.

Conclusion

The FTP server is using an old version of vsFTPd.

---

6. Challenge 4 – Anonymous FTP Login
Purpose

To test whether FTP allows anonymous access.

Command
ftp 10.0.2.15

Login:

Username: anonymous
Password: anonymous
<img width="454" height="72" alt="Screenshot 2026-04-29 164558" src="https://github.com/user-attachments/assets/1d568ea8-8c35-40a7-a41d-a15e900b5d52" />

Explanation

Some FTP servers allow guest access without credentials.

Findings

Anonymous login successful.

Reason

This can expose files to unauthorized users.

Conclusion

FTP anonymous access is enabled.

---

7. Challenge 5 – Version Detection
Purpose

To detect versions of running services.

Command
nmap -sV 10.0.2.15
<img width="752" height="161" alt="Screenshot 2026-04-29 165028" src="https://github.com/user-attachments/assets/a2f89b1a-4bd9-4c70-8606-4722996a7d04" />


Explanation

The -sV option identifies software names and versions.

Findings
vsFTPd 2.3.4
OpenSSH
Apache HTTP Server
Reason

Old versions may contain vulnerabilities.

Conclusion

Multiple outdated services were detected.

---

8. Challenge 6 – OS Detection
Purpose

To determine the operating system.

Command
nmap -O 10.0.2.15
<img width="715" height="203" alt="Screenshot 2026-04-29 165524" src="https://github.com/user-attachments/assets/e3be8ddc-9f1a-4c5f-a75d-17016ef6f646" />


Explanation

Nmap compares network fingerprints with known OS signatures.

Findings

Linux Kernel 2.6.x

Reason

Kernel versions help determine patch status.

Conclusion

The target system is running Linux.

---

9. Challenge 7 – Enum4linux
Purpose

To gather SMB information such as users and shares.

Command
enum4linux -a 10.0.2.15
<img width="673" height="59" alt="Screenshot 2026-04-29 165642" src="https://github.com/user-attachments/assets/5c7cd7e8-dd4d-4c40-a24c-0c33bbe99778" />


Explanation

Enum4linux performs SMB and NetBIOS enumeration.

Findings
Workgroup name
Shares
User information
Reason

SMB misconfiguration may reveal sensitive information.

Conclusion

The SMB service leaks useful enumeration data.

---

10. Challenge 8 – NFS Exports
Purpose

To identify shared NFS folders.

Command
showmount -e 10.0.2.15
<img width="290" height="51" alt="Screenshot 2026-04-29 165732" src="https://github.com/user-attachments/assets/692cdbd1-79d5-498c-ace9-9ed7af190c81" />


Explanation

This command displays exported NFS directories.

Findings
/home *
Reason

Open file shares may expose data.

Conclusion

NFS exports are available on the target.

---

11. Challenge 9 – RPC Info
Purpose

To identify active RPC services.

Command
rpcinfo -p 10.0.2.15
<img width="901" height="475" alt="Screenshot 2026-04-29 170330" src="https://github.com/user-attachments/assets/21a3c43e-224d-4caa-9a4a-fb80f8fe6469" />


Explanation

RPC is used for network services such as NFS.

Findings
portmapper
mountd
nfs
Reason

RPC services increase attack surface.

Conclusion

Multiple RPC services are enabled.

---

12. Challenge 10 – SNMPWalk
Purpose

To collect system information via SNMP.

Command
snmpwalk -v1 -c public 10.0.2.15
<img width="302" height="60" alt="Screenshot 2026-04-29 170443" src="https://github.com/user-attachments/assets/613588b8-b55e-48b4-a43e-e235ca0f3a6a" />

Explanation

SNMP community string public is commonly used as a default credential.

Findings
sysName
sysDescr
Interfaces
Uptime
Reason

Default SNMP credentials expose system data.

Conclusion

SNMP reveals sensitive information.

13. Final Conclusion

This lab successfully demonstrated enumeration techniques against the Metasploitable 2 machine using IP address 10.0.2.15.

The target system exposed many services including FTP, SMB, RPC, SNMP, and HTTP. Several insecure configurations were found such as anonymous FTP login, outdated software versions, and excessive information disclosure.

Enumeration is an important first step in vulnerability analysis because it provides technical intelligence before exploitation. Proper hardening, patching, and service restriction are recommended to improve system security.

References
Nmap Official Documentation
Kali Linux Tools Documentation
Metasploitable 2 Guide
OWASP Testing Guide
