# Vulnerability Assessment Lab — Parth

**Summary**  
This project documents a controlled vulnerability assessment performed in a lab environment on DVWA (Damn Vulnerable Web App). The goal is to demonstrate basic reconnaissance and web vulnerability testing using open-source tools.

**Environment**  
- Attacker: Kali Linux (VM)  
- Target: DVWA (Docker) on `127.0.0.1:8080` (isolated lab)  
- Network: Host-only / Internal (no external scanning)

**Tools Used**  
- Nmap — network & service discovery  
- Nikto — web server scanning  
- Burp Suite — proxy & manual request manipulation  
- Browser / DVWA UI — manual XSS demonstration

**What’s included**  
- `evidence/` — raw scan outputs and screenshots  
- `scripts/` — simple scan scripts to reproduce results  
- `REPORT_Vulnerability_Assessment.pdf` — full report with findings and remediation

**Quick reproduction**  
1. Start DVWA (Docker or manual).  
2. Run Nmap:
   ```bash
   ./scripts/nmap_scan.sh 127.0.0.1
3. Run Nikto:
   ```bash
   ./scripts/nikto_scan.sh 127.0.0.1:8080
4. Inspect Proofs for Outputs and Screenshots.

Disclaimer
This repository is for educational purposes only. Do not use these techniques on systems you do not own or have explicit permission to test.


---

# 3) Short, professional Report Summary snippet 
**Executive Summary:**  
> This vulnerability assessment targeted a lab instance of DVWA to practice reconnaissance and basic web testing. Using Nmap and Nikto we enumerated exposed services and HTTP server information; with Burp Suite and manual form testing we demonstrated Cross-Site Scripting (XSS) and identified input handling weaknesses. Each finding includes evidence, impact assessment, and recommended remediation. Automated exploitation (sqlmap) is planned as follow-up work.

---

# 4) Findings — 

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

**Finding D — Manual Testing / Burp Evidence**  
- **What:** Burp Repeater/Proxy captures show form parameters and behavior on tampered inputs (used for manual verification).  
- **Evidence:** `evidence/burp_requests/*` files, `evidence/screenshots/burp_intercept.png`  
- **Recommendation:** Harden input validation and add server-side checks.

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



