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
