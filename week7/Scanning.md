First Challenge – packet1.pcap
The first PCAP task was relatively simple, especially for participants who already understood Base64 encoding and basic packet capture analysis. However, for those without prior experience in network traffic investigation, it could take more time. During the competition, several players needed longer to complete it, which is understandable since CTF participants often come from different technical backgrounds and skill levels.
My first step would be to inspect the type of traffic contained in the capture file. After reviewing the packets, it becomes clear that every packet uses the ICMP protocol. Because the packet sizes are very small, ICMP tunneling can likely be ruled out. This suggests that the payload may simply contain readable text data.
When working with text hidden inside packet captures, the strings command is useful:
<img width="929" height="363" alt="Screenshot 2026-04-29 141554" src="https://github.com/user-attachments/assets/ddceeff2-51e3-451d-a7b4-21074e76a931" />

strings packet1.pcap
<img width="234" height="157" alt="Screenshot 2026-04-29 141704" src="https://github.com/user-attachments/assets/71be9ac2-eb53-4106-8b0b-e351cd836f4c" />

If the file contained excessive repeated traffic, filtering the packets first would help reduce clutter and make analysis easier.
Even if someone did not immediately recognize Base32 or Base64 encoding, tools such as ChatGPT or other decoders could help identify the encoded text. Since the event theme involved artificial intelligence, using AI assistance would be fitting. Once decoded, the hidden flag is revealed.
Flag: UCTF2023{ai_is_cool}
<img width="644" height="159" alt="Screenshot 2026-04-29 141835" src="https://github.com/user-attachments/assets/c27c4af7-583c-4de6-811c-7416b35ca7e2" />

Second Challenge – packet2.pcap
I considered the second challenge to be of moderate difficulty, although some players may still view it as easy. This packet capture included a combination of ICMP traffic, TCP communication through Netcat, and FTP sessions. Some participants spent extra time locating the FTP traffic, which slowed their progress. Using the strings command earlier would have made discovery faster.
After examining the packet capture, ICMP traffic appears again along with TCP and FTP packets. Since the ICMP packets have similar byte sizes to the first challenge, they can likely be ignored once more. Because FTP transmits data in plaintext, extracting readable strings is an effective method.
Reviewing the output shows a successful transfer of a file named:
<img width="932" height="822" alt="Screenshot 2026-04-29 150350" src="https://github.com/user-attachments/assets/6ce93e79-cc76-41a0-95ec-393c8e1aa841" />
global_thermonuclear_war.gamerules.txt
Inside the text file is a link to a Google Docs document. Opening it reveals a file called Club Tux, containing images made up of unusual symbols.
In many CTF challenges, creators provide hints for solving puzzles. Two clues were intentionally placed here:


A tic-tac-toe pattern mentioned in the conversation


The filename Club Tux

<img width="792" height="669" alt="Screenshot 2026-04-29 152336" src="https://github.com/user-attachments/assets/2e3626ad-a8fe-4388-bf11-e05db375b792" />

Since Tux is the Linux penguin mascot, searching for “Club Penguin secret code” points to a Tic-Tac-Toe cipher.
Using an online decoder and uploading the symbol images translates the message into:
EX MACHINA AVA
Flag: EXMACHINAAVA

Question 3: Interpret an Nmap Output
PORT   STATE SERVICE      VERSION21/tcp open  ftp          vsftpd 2.3.422/tcp open  ssh          OpenSSH 5.3p180/tcp open  http         Apache 2.2.8139/tcp open  netbios-ssn445/tcp open  microsoft-ds Windows 7 Professional 7601 SP1
1. Port Analysis (From an Attacker’s Perspective)
Port 21 – FTP (vsftpd 2.3.4)


Allows file upload and download access


May permit anonymous login if poorly configured


Contains a known backdoored release that could enable shell access


Port 22 – SSH (OpenSSH 5.3p1)


Used for remote administrative login


Vulnerable to brute-force password attacks


Older versions may support weak encryption methods


Port 80 – HTTP (Apache 2.2.8)


Main web service entry point


Possible risks include:


SQL Injection


Remote code execution


File upload abuse


Directory traversal




Port 139 – NetBIOS


Can expose shared files, printers, and usernames


Useful for enumeration


May assist SMB relay attacks


Port 445 – SMB (Windows 7 SP1)


Used for file sharing and remote administration


Enables movement across internal systems


High risk of remote exploitation



2. Likely Vulnerabilities
vsftpd 2.3.4


Associated with CVE-2011-2523


Backdoored version may grant shell access


OpenSSH 5.3p1


Outdated software


Weak security controls compared to modern versions


Password brute-force attempts possible


Apache 2.2.8


End-of-life version


Multiple known security flaws depending on setup


Windows 7 SMB


Vulnerable to MS17-010 (EternalBlue)


SMB relay attacks possible


NTLM credential theft risk



3. Highest Risk Service
SMB – Port 445
Reason:


Supports unauthenticated remote code execution through EternalBlue


Can lead to complete system takeover with SYSTEM privileges


Wormable exploit used in WannaCry ransomware


Useful for lateral movement inside networks



4. Potential Attack Path

<img width="502" height="196" alt="Screenshot 2026-04-29 152547" src="https://github.com/user-attachments/assets/a51ff1f1-6c42-4d64-86f0-f05ef88f02ef" />
Answer: - Linux / Unix Operating System - Reason: Linux default TTL = 64

<img width="587" height="400" alt="Screenshot 2026-04-29 152648" src="https://github.com/user-attachments/assets/0c1b8417-1c26-4467-b5d8-cbd4c9e802f0" />
Answer: - Linux / Unix Operating System - Reason: Linux default TTL = 64.

<img width="904" height="178" alt="Screenshot 2026-04-29 152735" src="https://github.com/user-attachments/assets/4717a80c-093a-4ca2-9fb6-fd1f7ee1f4f8" />
Answer: - Network Device (Router / Cisco device) - Reason: Networking devices commonly use TTL 255


5. Remediation Plan
Critical Actions

<img width="946" height="310" alt="Screenshot 2026-04-29 154038" src="https://github.com/user-attachments/assets/da9be65a-ce6b-4187-a91e-e0d705f3206b" />

Apply MS17-010 patch or upgrade Windows 7 immediately


Disable SMBv1 protocol


Service Hardening


Replace or update vsftpd


Upgrade OpenSSH to latest stable release


Upgrade Apache to 2.4+


Network Protection


Block ports 21, 139, and 445 from external access


Restrict SSH to trusted IP addresses only


Disable unused services


Authentication Security


Enforce strong passwords


Disable anonymous FTP


Use SSH keys instead of passwords


Monitoring


Deploy IDS/IPS solutions


Monitor login attempts


Run regular vulnerability scans


Conclusion
This system exposes several outdated services. The most severe issue is SMB on port 445 due to its ability to allow remote code execution and total compromise. Immediate patching and service hardening are strongly recommended.

Question 4: Identify the OS (TTL Fingerprinting)
Image 1
Answer: Linux / Unix
Reason: Default TTL is commonly 64
Image 2
Answer: Linux / Unix
Reason: TTL value of 64 usually indicates Linux-based systems
Image 3
Answer: Router / Network Device
Reason: Many networking devices such as Cisco systems commonly use TTL 255

Question 5: Nessus Vulnerability Analysis – Ghostcat (CVE-2020-1938)
1. Affected Port
8009/tcp
This is the default port used by the AJP connector.
2. Affected Protocol
AJP (Apache JServ Protocol)
AJP is commonly used for communication between Apache HTTP Server and Apache Tomcat.
3. CVSS Score
9.8 – Critical
The vulnerability is considered critical because it may allow arbitrary file disclosure and, in some cases, remote code execution.
4. Public Exploits Available
Yes, public exploits exist, including:
auxiliary/admin/http/tomcat_ghostcat


Python Proof-of-Concept scripts


Attackers may use Ghostcat to:


Read sensitive files such as /WEB-INF/web.xml


Gather configuration details


Escalate to remote code execution if writable paths are exposed


5. CVE Identifier
CVE-2020-1938
Summary


Port: 8009/tcp


Protocol: AJP


Severity: Critical (9.8)


Exploit Status: Publicly available


CVE: CVE-2020-1938

