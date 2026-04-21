🛡️ BlueMoon VulnHub – Penetration Testing Report
🎯 Objective

The goal of this assessment was to identify vulnerabilities in the BlueMoon VM, gain initial access, and escalate privileges to root in a controlled lab environment.

1. 🧭 Network Discovery
<img width="627" height="280" alt="Screenshot 2026-04-21 152138" src="https://github.com/user-attachments/assets/a9bc7b29-10b2-4f7c-9598-562be748ff34" />
<img width="627" height="281" alt="Screenshot 2026-04-21 152326" src="https://github.com/user-attachments/assets/c271acd4-433e-46b2-b05b-3de04efb0bcd" />

📌 Purpose:

This command performs a ping sweep to discover live hosts in the subnet.

🧠 Explanation:
-sn → disables port scanning, only checks if host is alive
/24 → scans all 256 IP addresses in the network range
🔎 Result:
Found multiple hosts:
192.168.56.100
192.168.56.101
192.168.56.102 (Kali machine)

👉 Target identified: 192.168.56.101


2. 🔎 Port Scanning (Service Enumeration)
<img width="620" height="376" alt="Screenshot 2026-04-21 155537" src="https://github.com/user-attachments/assets/aa9b73d3-ecd9-42be-b178-16a2e628fb82" />

📌 Purpose:

To identify open ports and running services.

🧠 Explanation:
-sC → runs default Nmap scripts (basic vulnerability checks)
-sV → detects service versions
🔎 Expected result:
HTTP (port 80)
SSH (port 22)
FTP (port 21) (depending on VM state)

👉 This confirms a multi-service attack surface


3. 🌐 Web Enumeration
🔍 Command used:
<img width="622" height="379" alt="Screenshot 2026-04-21 162755" src="https://github.com/user-attachments/assets/bc85daa5-fe67-4ab6-851f-e1f5c0fca498" />

📌 Purpose:

To discover hidden directories on the web server.

🧠 Explanation:
dir → directory brute force mode
-w → wordlist for guessing hidden paths
🔎 Result:
/hidden_text (Status: 200)

👉 This is a hidden endpoint containing further clues


4. 🌐 Hidden Content Discovery (Browser-Based Method)
   
📌 Method used:
Instead of using curl, the researcher accessed the web application via a browser:

<img width="1033" height="388" alt="Screenshot 2026-04-21 173329" src="https://github.com/user-attachments/assets/5e349fd4-d6a7-4c47-b373-080d932a8924" />

Then used:

<img width="1044" height="770" alt="Screenshot 2026-04-21 173411" src="https://github.com/user-attachments/assets/7e39d84b-4bd7-41e9-a35a-ae51d655908d" />


View Page Source / Inspect Element
Located a hidden hyperlink pointing to a QR code image
🧠 Explanation:
“Inspect Element” allows viewing the raw HTML structure
Hidden or less obvious links can be found that are not emphasized in the UI
This simulates real-world attacker behavior using a browser-based recon approach
🔎 Finding:

A hidden link was discovered:

<img width="1388" height="836" alt="Screenshot 2026-04-21 173430" src="https://github.com/user-attachments/assets/84bd193b-7f2a-43a4-b229-97b36664bee8" />

This indicated a steganographic / encoded information file (QR code image)

📌 Action taken:
The image was opened in the browser
QR code was scanned using a mobile or QR scanner tool
🔓 Result:

The QR code revealed a script containing:

FTP credentials:
Username: userftp
Password: ftpp@ssword


5. 📂 FTP Access
🔍 Command used:

192.168.56.101
📌 Purpose:
<img width="292" height="186" alt="Screenshot 2026-04-21 173829" src="https://github.com/user-attachments/assets/c09682eb-7858-45b8-ab14-7b0004ebf82c" />

Login to FTP service using discovered credentials.

🧠 Explanation:
ftp → file transfer protocol client
🔎 Result:

Access granted and files retrieved:

information.txt
p_lists.txt


6. 📄 File Analysis
🔍 Command:
<img width="579" height="95" alt="Screenshot 2026-04-21 173933" src="https://github.com/user-attachments/assets/6e1bca76-0a4d-4366-90bd-ebce00ac0883" />
<img width="1880" height="209" alt="Screenshot 2026-04-21 174158" src="https://github.com/user-attachments/assets/63d7e017-38d5-4eec-8480-916217d4929a" />


📌 Purpose:

Read downloaded files.
<img width="994" height="681" alt="Screenshot 2026-04-21 174356" src="https://github.com/user-attachments/assets/d0fef465-12f5-4613-90a7-dec29e3456aa" />

🧠 Findings:
information.txt
Mentions user: robin
Indicates weak password usage
p_lists.txt
Contains password wordlist for brute force attack

👉 This confirms credential reuse vulnerability


7. 🔐 SSH Brute Force Attack
🔍 Command used:
<img width="1665" height="241" alt="Screenshot 2026-04-21 174537" src="https://github.com/user-attachments/assets/b36b61e7-af8f-44d4-89cc-a3ba02105402" />


📌 Purpose:

Automated SSH login attempt using password list.

🧠 Explanation:
-l → single username (robin)
-P → password list
ssh:// → target service
🔎 Result:
login: robin
password: k4rv3ndh4nh4ck3r


8. 💻 SSH Login
🔍 Command:
<img width="653" height="377" alt="Screenshot 2026-04-21 174712" src="https://github.com/user-attachments/assets/838d0a5f-91f0-4ff5-b4c9-96a1eb06f406" />

📌 Purpose:

Gain shell access to the system.

🧠 Explanation:
ssh → secure remote login protocol
🔎 Result:

✔ User access obtained


9. 🏁 User Flag Retrieval
🔍 Command:
<img width="345" height="147" alt="Screenshot 2026-04-21 174744" src="https://github.com/user-attachments/assets/126960df-30f7-47e6-adb1-ccaf1f96ec2a" />

📌 Purpose:

Retrieve user-level flag.
