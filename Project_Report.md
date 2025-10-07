
---

**Executive Summary:**  
> This vulnerability assessment targeted a lab instance of DVWA to practice reconnaissance and basic web testing. Using Nmap and Nikto we enumerated exposed services and HTTP server information; with Burp Suite and manual form testing we demonstrated Cross-Site Scripting (XSS) and identified input handling weaknesses. Each finding includes evidence, impact assessment, and recommended remediation. Automated exploitation (sqlmap) is planned as follow-up work.

---

# Findings — 

**Finding A — Service & Port Discovery (Nmap)**  
- **What:** Nmap scan revealed HTTP service on port 80/8080 and open SSH/other test ports on the lab VM.  
- **Evidence:** `evidence/nmap_top100.txt`, `evidence/screenshots/nmap_screenshot.png`  
- **Risk:** Low–Medium (exposed services increase attack surface)  
- **Recommendation:** Disable unused services, restrict access via firewall, and keep software updated.

**Finding B — Web Server Misconfigurations (Nikto)**  
- **What:** Nikto reported server headers and possible directory information disclosures (and known outdated modules).  
- **Evidence:** `evidence/nikto_report.txt`, `evidence/screenshots/nikto_screenshot.png`  
- **Risk:** Medium (information disclosure aids attackers).  
- **Recommendation:** Hide server version info, disable directory listing, and patch web server.

**Finding C — Cross-Site Scripting (XSS)**  
- **What:** Stored/reflected XSS demonstrated on DVWA XSS pages by injecting `<script>alert('XSS')</script>`.  
- **Evidence:** `evidence/screenshots/xss_alert.png`, Burp saved request.  
- **Risk:** Medium–High (session theft, phishing).  
- **Recommendation:** Implement proper output encoding & input validation; set Content Security Policy (CSP).

---

**Nmap:**
   ```bash
   nmap -sS -T4 --top-ports 100 -oN Proofs/nmap_top100.txt 127.0.0.1
   nmap -sV -O -p- --min-rate 1000 -oN Proofs/nmap_full.txt 127.0.0.1
   ```
**Nikto:**
   ```bash
   nikto -h http://127.0.0.1:8080 -output Proofs/nikto_report.txt
   ```



