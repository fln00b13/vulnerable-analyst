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

### 📸 Screenshot

*(Insert Screenshot here)*

---

## 🌐 2. Discovering `/island`

From the scan, I found the `/island` directory with a valid status.

I accessed it via browser:

```
http://10.X.X.X/island
```

### 📸 Screenshot

*(Insert Screenshot here)*

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

### 📸 Screenshot

*(Insert Screenshot here)*

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

### 📸 Screenshot

*(Insert Screenshot here)*

---

## 🔐 5. Finding FTP Password

I attempted further enumeration but found nothing, so I checked the **source code again** and found encoded text.

### 📸 Screenshot

*(Insert Screenshot here)*

I decoded it using **CyberChef**:

* Used **Base58 decoding**

### 📸 Screenshot

*(Insert Screenshot here)*

👉 This revealed the **FTP password**

---

## 📂 6. Finding SSH Password File

I accessed FTP:

```bash
ftp 10.X.X.X
```

### 📸 Screenshot

*(Insert Screenshot here)*

I found **3 PNG files** and downloaded them.

### 📸 Screenshot

*(Insert Screenshot here)*

Then I extracted hidden data:

```bash
steghide extract -sf aa.jpg
```

Unzipped and viewed contents:

```bash
unzip ss.zip
cat passwd.txt
```

### 📸 Screenshot

*(Insert Screenshot here)*

👉 Found the SSH password file.

---

## 👤 7. Capturing User Flag

I attempted SSH login:

```bash
ssh username@10.X.X.X
```

Initially failed, but after retrying with correct credentials, I successfully logged in.

### 📸 Screenshot

*(Insert Screenshot here)*

👉 Retrieved **user.txt**

---

## 👑 8. Privilege Escalation & Root Flag

While exploring, I found `/bin/sh` access.

I researched privilege escalation using **pkexec**, which allowed escalation to root.

### 📸 Screenshot

*(Insert Screenshot here)*

Then:

```bash
cat root.txt
```

### 📸 Screenshot

*(Insert Screenshot here)*

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
