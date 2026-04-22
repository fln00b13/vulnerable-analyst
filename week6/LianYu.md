# TryHackMe – Lian Yu Writeup

## 🔗 Room Link

https://tryhackme.com/room/lianyu

---

## 📌 Project Overview

This project documents the penetration testing process performed on the TryHackMe **“Lian Yu”** room as part of Week 6 lab activities.

### 🎯 Objectives

* Perform network reconnaissance
* Identify exposed services
* Enumerate web directories
* Decode hidden information
* Exploit the target system
* Capture the User Flag
* Capture the Root Flag
* Document methodology and findings

---

## 🔍 1. Initial Scanning

First, I started with directory enumeration using Gobuster:

```bash
gobuster dir -u http://10.X.X.X -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

<img width="1916" height="791" alt="Screenshot 2026-04-22 191049" src="https://github.com/user-attachments/assets/aadddce2-16bb-4535-9409-b9c4cf5d3880" />


---

## 🌐 2. Discovering `/island`

From the scan, I found the `/island` directory with a valid status.

I accessed it via browser:

```
http://10.X.X.X/island
```

<img width="596" height="239" alt="Screenshot 2026-04-22 191117" src="https://github.com/user-attachments/assets/44aba317-6548-401e-856d-d74984ecf8b8" />

---

## 🔎 3. Finding Web Directory (Answer: 2100)

To answer the question:

> What is the Web Directory you found?

I ran:

```bash
gobuster dir -u http://10.X.X.X/island -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

I discovered:

```
/2100
```

<img width="1780" height="348" alt="Screenshot 2026-04-22 194444" src="https://github.com/user-attachments/assets/837c05e1-2743-4bbd-940a-9a7180e1b9ca" />

---

## 📄 4. Finding File Name (Answer: green_arrow.ticket)

Next, I enumerated inside `/2100`:

```bash
gobuster dir -u http://10.X.X.X/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Initially, nothing appeared useful. So I checked the **page source code** and found a clue.

### 📸 Screenshot

*(Insert Screenshot here)*

Then I used file extension fuzzing:

```bash
gobuster dir -u http://10.X.X.X/island/2100 -x ticket -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Found:

```
green_arrow.ticket
```

<img width="686" height="106" alt="Screenshot 2026-04-22 195409" src="https://github.com/user-attachments/assets/47382768-9038-4eba-b3af-b7bc5b3118fb" />
<img width="1596" height="934" alt="Screenshot 2026-04-22 195319" src="https://github.com/user-attachments/assets/13387bc6-1269-4fe6-b0c5-079fb2f0410a" />

---

## 🔐 5. Finding FTP Password

I attempted further enumeration but found nothing, so I checked the **source code again** and found encoded text.

<img width="699" height="165" alt="Screenshot 2026-04-22 195619" src="https://github.com/user-attachments/assets/24831dc6-0c1e-45ca-9793-b8ac876c8d32" />


I decoded it using **CyberChef**:

* Used **Base58 decoding**

<img width="665" height="100" alt="Screenshot 2026-04-22 195913" src="https://github.com/user-attachments/assets/877825eb-0eb4-4f5b-8af3-626df41ec29a" />
<img width="953" height="507" alt="Screenshot 2026-04-22 195834" src="https://github.com/user-attachments/assets/77dbc724-c67a-42e4-817b-c5929beae12a" />


👉 This revealed the **FTP password**

---

## 📂 6. Finding SSH Password File

I accessed FTP:

```bash
ftp 10.X.X.X
```

<img width="932" height="670" alt="Screenshot 2026-04-22 200456" src="https://github.com/user-attachments/assets/4338dad0-7508-45aa-b319-8dac9773fdcf" />


I found **3 PNG files** and downloaded them.

Then I extracted hidden data:

```bash
steghide extract -sf aa.jpg
```

Unzipped and viewed contents:

```bash
unzip ss.zip
cat passwd.txt
```

<img width="1789" height="381" alt="Screenshot 2026-04-22 202516" src="https://github.com/user-attachments/assets/0d9e59eb-ddd1-418a-8753-514ae9429879" />


👉 Found the SSH password file.

---

## 👤 7. Capturing User Flag

I attempted SSH login:

```bash
ssh username@10.X.X.X
```

Initially failed, but after retrying with correct credentials, I successfully logged in.

<img width="597" height="487" alt="Screenshot 2026-04-22 203234" src="https://github.com/user-attachments/assets/03c9feea-311f-4317-9aaa-c3b1d89052b5" />


👉 Retrieved **user.txt**

---

## 👑 8. Privilege Escalation & Root Flag

While exploring, I found `/bin/sh` access.

I researched privilege escalation using **pkexec**, which allowed escalation to root.

<img width="1782" height="579" alt="Screenshot 2026-04-22 203545" src="https://github.com/user-attachments/assets/2ad2bd54-58a8-44a2-b8a5-3774bbf50b50" />


Then:

```bash
cat root.txt
```

<img width="1919" height="1011" alt="Screenshot 2026-04-22 204131" src="https://github.com/user-attachments/assets/c52699ab-3bce-4f8c-b6a1-d046c54fb78b" />


👉 Retrieved **root flag**

---

## ✅ Conclusion

This lab demonstrated:

* Web enumeration techniques
* Directory brute forcing
* Source code analysis
* Encoding/decoding (Base58)
* Steganography (steghide)
* FTP exploitation
* SSH access
* Privilege escalation using pkexec

---

## 🎉 Final Notes

This was a great hands-on lab that combined multiple penetration testing techniques in a structured scenario.

**Thank you :)**
