# Vulnerability Assessment Lab — Parth

**Summary**  
This project documents a controlled vulnerability assessment performed in a lab environment on DVWA (Damn Vulnerable Web App). The goal is to demonstrate basic reconnaissance and web vulnerability testing using open-source tools.

---

## Environment
- **Attacker:** Kali Linux (VM)  
- **Target:** DVWA (Docker) on `127.0.0.1:8080` (isolated lab)  
- **Network:** Host-only / Internal (no external scanning)

## Tools Used
- **Nmap** — network & service discovery  
- **Nikto** — web server scanning  
- **Burp Suite** — proxy / manual testing & Intruder  
- **sql Injection** — optional automated SQLi enumeration  
- **Browser / DVWA UI** — manual XSS / SQLi demonstration

## What’s included
- `proofs/` — raw scan outputs and screenshots  
- `REPORT_Vulnerability_Assessment.pdf` — full report with findings and remediation

---

## Quick reproduction
1. Start DVWA (Docker or manual).  
2. Run Nmap:
```bash
./scripts/nmap_scan.sh 127.0.0.1

---

## Quick reproduction
1. Start DVWA (Docker or manual).  
2. Run Nmap:
```bash
./scripts/nmap_scan.sh 127.0.0.1
```
3. Run Nikto:
```bash
./scripts/nikto_scan.sh 127.0.0.1:8080
```
4. Inspect `evidence/` for outputs and screenshots.

---

## How to start DVWA using Docker (lab-style)

Below is the simplified, lab-friendly set of commands that matches the quick process used on a laptop.

### Quick Docker commands (lab-style)

1. **Install Docker (if not installed)**
```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

2. **Pull the DVWA image**
```bash
sudo docker pull vulnerables/web-dvwa
```

3. **Run the DVWA container**
```bash
sudo docker run --rm -it -p 127.0.0.1:8080:80 vulnerables/web-dvwa
```

4. **Access DVWA in browser:**
```
http://127.0.0.1:8080
```

5. **Login credentials:**
- Username: `admin`
- Password: `password`

6. **Initialize the database:**
Once logged in → click **"Create / Reset Database"** under the **Setup** tab.

---

## DVWA first-time setup & notes
1. Visit `http://127.0.0.1:8080` in your browser.  
2. If the database initialization fails, check container logs with:
```bash
docker logs <container_id_or_name>
```

---

## Burp capture (localhost gotchas)

If Burp is not capturing requests to http://127.0.0.1:8080, this is a common browser/localhost proxy issue. Try one of these fixes:

Use a hostname instead of localhost (recommended):
```bash
sudo sh -c "echo '127.0.0.1 dvwa.local' >> /etc/hosts"
# then browse: http://dvwa.local:8080
```
This prevents browsers from bypassing the proxy for localhost.


## Troubleshooting
- **Port already in use:** If `127.0.0.1:8080` is occupied, stop the service using that port or change the mapping (e.g., `-p 127.0.0.1:9090:80`).  
- **Database errors:** Use the DVWA UI `Create / Reset Database` button. If that fails, check container logs (`docker logs <container_id_or_name>`).  
- **Permission or networking issues in VM:** Ensure Docker is installed and that your VM networking allows host-only connections to `127.0.0.1`. For bridged or NAT networking, adapt accordingly.  
- **Persistent storage:** The `vulnerables/web-dvwa` image is ephemeral. If you want to persist changes, mount a host volume to `/var/www/html` in the container.

---
## SQL Injection assessment (DVWA) — summary & steps

Target: http://dvwa.local:8080/vulnerabilities/sqli/
Parameter: id (GET)

**Quick manual steps (Burp Repeater):**

Intercept a sample request in Burp Proxy (submit id=1 from DVWA).

Right-click → Send to Repeater. Use Repeater to test payloads (one at a time) and click Go to observe responses.

**Payloads to test (manual)**

`1' OR '1'='1`— boolean true test (should return more rows)

`1' `— error-based test (may reveal DB error)

`1' UNION SELECT NULL--` - then increase NULL count until no error — to find UNION column count

`1' OR SLEEP(5)-- -` — time-based test (response delayed ~5s)

**Intruder workflow (optional):**

Send the request to Intruder, mark id as position, use Sniper with a small SQL payload list, run and look for differences in Response Length / Time.

**YOU CAN CHECK PROOF/SQLI FOLDER FOR THE SCREENSHOTS**

## Notes & Disclaimer
This repository is for educational purposes only. Do not use these techniques on systems you do not own or do not have explicit permission to test.

---

## Contact
Lab owner: **Parth**
