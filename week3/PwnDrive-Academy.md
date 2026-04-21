🧾 🔐 Penetration Testing Report – PwnDrive (10.150.150.11)

1. Reconnaissance
The initial reconnaissance phase was performed using Nmap to identify open ports, services, and potential attack vectors on the target system
<img width="854" height="834" alt="Screenshot 2026-04-22 021011" src="https://github.com/user-attachments/assets/95aa2d3c-18d4-4199-b8bd-733a00f707de" />
📊 Results:

The scan revealed multiple open services:

21/tcp – FTP
80/tcp – HTTP (Apache httpd 2.4.46, PHP 7.4.9)
135/tcp – MSRPC
139/tcp – NetBIOS
445/tcp – SMB
443/tcp – HTTPS
1433/tcp – Microsoft SQL Server
3306/tcp – MySQL
3389/tcp – RDP
🧠 Analysis:

The target is a Windows Server (likely 2008 R2) hosting:

A web application (PwnDrive)
Database services (MSSQL + MySQL)
File sharing services (SMB, FTP)
Remote access (RDP)

👉 This indicates a large attack surface with multiple entry points


2. Web Application Enumeration

Directory brute force was performed to identify hidden endpoints within the web application.

🔍 Command used:
<img width="843" height="882" alt="Screenshot 2026-04-22 032409" src="https://github.com/user-attachments/assets/79d9c58b-05dd-4a0e-b5e6-599693cefff4" />

📂 Discovered directories:
/admin
/upload
/inc
/utils
/components
🧠 Analysis:

The presence of:

/admin → administrative panel
/upload → file upload functionality

suggests potential for:

authentication bypass
file upload exploitation
sensitive file exposure


3. Authentication Testing (SQL Injection)

The /admin login page was tested for authentication weaknesses.

🔍 Observations:
No CAPTCHA
No rate limiting
No account lockout mechanism
💉 Exploitation attempt:

A SQL injection authentication bypass payload was used:
<img width="1123" height="724" alt="Screenshot 2026-04-22 032708" src="https://github.com/user-attachments/assets/f7f5e5d5-e59b-4930-86ee-88c406bd380c" />
Username: admin
Password: admin' #

🧠 Result:
Successful bypass of authentication allowed access to the admin panel.


4. Post-Exploitation (Web Application)

After gaining access, the following functionalities were discovered:
<img width="689" height="342" alt="Screenshot 2026-04-22 032603" src="https://github.com/user-attachments/assets/8c868d38-ad45-4cbb-b0ce-b0760ab9e6a9" />

File upload
Folder creation
File download / sharing / deletion

🧠 Analysis:

The upload functionality presents a high-risk attack vector, potentially allowing:

Remote code execution (RCE)
Web shell upload
Privilege escalation


5. SMB Vulnerability Assessment

SMB vulnerability scanning was performed using Nmap scripts.
<img width="811" height="470" alt="Screenshot 2026-04-22 033541" src="https://github.com/user-attachments/assets/f763dcbe-74c2-4e17-ac0a-02183750b90c" />
🧠 Result:

No direct MS17-010 (EternalBlue) confirmation was clearly validated in the scan output.

👉 Important correction: EternalBlue should NOT be assumed unless explicitly detected as vulnerable.


6. Exploitation Path
<img width="936" height="767" alt="Screenshot 2026-04-22 033933" src="https://github.com/user-attachments/assets/111100e9-0455-4ff6-a5a1-8db2d27e3db6" />
<img width="938" height="748" alt="Screenshot 2026-04-22 034258" src="https://github.com/user-attachments/assets/add8ba6f-d69b-4470-be86-1950a6c068fa" />
<img width="921" height="411" alt="Screenshot 2026-04-22 034446" src="https://github.com/user-attachments/assets/4c55fb15-7c4c-45bf-b209-585d764bdd18" />
<img width="624" height="197" alt="image" src="https://github.com/user-attachments/assets/d427f6c2-e6db-4ed2-9889-13b5a5502cb6" />
<img width="438" height="693" alt="image" src="https://github.com/user-attachments/assets/03fb8e58-93d5-4409-ad45-d8833156d16c" />

⚠️ Note:
The exploitation path using Metasploit (EternalBlue) is not confirmed by scan output and should not be included unless vulnerability is verified.


✔️ Valid exploitation path (recommended):
🔵 Path A: Web Application Exploitation
/admin login bypass via SQL injection
/upload file upload exploitation
Web shell execution
🔵 Path B: SMB / Database Enumeration
SMB share enumeration
Credential extraction from config files
MSSQL/MySQL exploitation (if weak credentials exist)
