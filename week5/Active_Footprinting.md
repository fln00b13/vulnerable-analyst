Domain Enumeration
1. WHOIS
<img width="931" height="565" alt="image" src="https://github.com/user-attachments/assets/e3e0f659-95c6-4743-ac9a-5b98c8a1499e" />
What to find:

Registrar
Creation date
Expiry
Name servers


DNS Enumeration
1. NSLOOKUP
<img width="280" height="227" alt="image" src="https://github.com/user-attachments/assets/3e1ce828-06a8-451c-8ef6-f4ce1840ef1c" />

2. DIG
<img width="552" height="352" alt="image" src="https://github.com/user-attachments/assets/2ad981af-e03a-4c8d-a45f-e0351875f114" />
What to find:

IP address
Mail servers (MX)
Subdomains (if visible)

Subdomain Discovery
<img width="938" height="822" alt="image" src="https://github.com/user-attachments/assets/511eb612-a468-411f-b2ca-3d6fc5eac078" />


Technology Footprinting
1. Buildwith
<img width="935" height="971" alt="Screenshot 2026-04-22 130154" src="https://github.com/user-attachments/assets/f92e23a8-ce55-4844-a967-e16a4cc5f367" />

2. Wappalyzer
<img width="622" height="680" alt="Screenshot 2026-04-22 130632" src="https://github.com/user-attachments/assets/ea9cf1cf-ab4c-4cfc-a0a6-bd01a934d334" />
What to find:
- Web server (Apache/Nginx)
- CMS (WordPress, etc.)
- Framework (PHP, Laravel, etc.)

Web Header and Basic Info
<img width="924" height="234" alt="Screenshot 2026-04-22 131109" src="https://github.com/user-attachments/assets/498475fd-6f0a-47f1-8ca9-89420d2ba364" />
What to find:
- Server type and version leakage
- Security headers:

1. X-Frame-Options
2. Content-Security-Policy (CSP)
3. Strict-Transport-Security (HSTS)

- Hidden directories
- Admin panels
- Disallowed paths

WAF Detection
<img width="619" height="376" alt="Screenshot 2026-04-22 131546" src="https://github.com/user-attachments/assets/0d62a650-1136-4da2-a984-7535e09dd734" />
What to find:

- Detect Cloudflare/AWS WAF
- Helps understand protecton level

SSL/TLS Info
<img width="1376" height="751" alt="image" src="https://github.com/user-attachments/assets/c2521d26-8dfd-4460-b9ac-d26660c020d3" />
What to find:

Certificate issuer
Expiry
Weak TLS version

