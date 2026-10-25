# Ethical Hacking Lab Tools


# ⚡ Ethical Hacking Lab Quick-Reference Manual (ICS1561)

---

## 🌐 01. Web & OSINT Tools (Browser-Based & Recon)

### 🔍 Google Dorking
* **Basic Search:** `site:microsoft.com`
* **Target Directories:** `site:microsoft.com careers`
* **File Harvesting:** `site:microsoft.com filetype:pdf`
* **Targeting Strings:** `site:microsoft.com "contact us"`
* **Exposing Data:** `site:microsoft.com inurl:"confidential"`

### 🎯 Shodan.io (IoT & Banner Grabbing)
* **Exploit Search:** `"vsftpd 2.3.4"`
* **Port & Geo Filter:** `port:21 country:"IN"`
* **Organization Filter:** `org:"Amazon.com" port:80`

### 🌐 Network Recon Web Tools
* **Robtex:** Access `robtex.com` ➔ Input `ssn.edu.in` ➔ Analyze shared infrastructure/IP history.
* **Nessus Essentials:** Log in (`natc`/`natc`) ➔ **New Scan** ➔ Target: Metasploitable IP ➔ Analyze vulnerabilities.

---

## 🪟 02. Windows Desktop & Kali GUI Tools

### 🛡️ Windows Systems
* **Sysmon Setup:** Run `./Sysmon64.exe -accepteula -i sysmonconfig-export.xml` to install and track endpoint telemetry.

### 🖼️ GUI OSINT & Analysis
* **FOCA:** Create project ➔ Target `google.com` ➔ Select all, search, download ➔ Right-click ➔ **Extract Metadata**.
* **Maltego:** Create new graph ➔ Drag infrastructure entity (e.g., DNS) ➔ Right-click ➔ **Run All Transforms**.

### 📡 Traffic Analysis & Exploitation
* **Wireshark:** Select Interface (`WiFi`) ➔ **Start Scan** ➔ Generate traffic (Open Brave & YouTube) ➔ **Stop Scan** ➔ Locate Brave frame ➔ Double-click to extract **Source & Destination IPs**.
* **Armitage:** Launch & exploit GUI:
  ```bash
  sudo service postgresql start
  sudo msfdb init
  sudo armitage
  ```
  * *GUI Steps:* Accept defaults ➔ **Hosts** ➔ **Add Hosts** ➔ Input Metasploitable IP ➔ **Hosts** ➔ **Nmap Scan** ➔ **Quick Scan** ➔ Right-click Target ➔ **Attack**.

---

## 💻 03. Terminal-Based Tools (Kali Linux)

### 🔍 Passive & Active Footprinting
* **WHOIS:** `whois google.com`
* **nslookup:** `nslookup -type=MX ezfoodz.com` or `nslookup -type=NS ezfoodz.com`
* **netdiscover (ARP Scanning):** `sudo netdiscover -r 192.168.58.0/24`

### 📋 Subdomain & OSINT Enumeration
* **theHarvester:** 
  ```bash
  theHarvester -d owasp.org -l 100 -b all
  theHarvester -d owasp.org -b hackertarget
  ```
* **Sublist3r / Subfinder:**
  ```bash
  sublist3r -d microsoft.com -t 10 -o subdomains.txt
  # Alternative setup and scan:
  sudo apt install subfinder && subfinder -d owasp.org
  ```

### 🛠️ Vulnerability Scanning & Web Assessment
* **Local Web Server Setup:** `python3 -m http.server 8000`
* **Nikto Web Scan:** `nikto -h http://127.0.0.1:8000`
* **Nmap Port Scanning:**
  ```bash
  # Quick local check
  nmap 127.0.0.1
  # OS Detection
  sudo nmap -O 127.0.0.1
  # Aggressive Scan (Syn Stealth, Version, OS, Scripts) saved to file
  sudo nmap -sS -sV -O -sC 8.8.8.8 -oN nat.txt
  ```



1. **[01_Browser_Based/](./01_Browser_Based/)** — Web & OSINT Tools  
   - [`Browser_Tools.md`](./01_Browser_Based/Browser_Tools.md) (Robtex, Shodan, Google Dorking, Nessus Essentials)
2. **[02_Windows_and_GUI/](./02_Windows_and_GUI/)** — Windows Desktop & Kali GUI Tools  
   - [`Windows_Tools.md`](./02_Windows_and_GUI/01_Windows_Tools/Windows_Tools.md) (FOCA, Maltego, Sysmon)
   - [`Other_GUI_Tools.md`](./02_Windows_and_GUI/02_Other_GUI_Tools/Other_GUI_Tools.md) (Wireshark, Armitage)
3. **[03_Terminal_Based/](./03_Terminal_Based/)** — Terminal-Based Tools (Kali Linux)  
   - [`Terminal_Tools.md`](./03_Terminal_Based/Terminal_Tools.md) (Full Practical Notes: Kali VM, WHOIS, nslookup, Nmap, theHarvester, Netdiscover, Sublist3r, Nikto)
   - [`Terminal_Commands_Summary.md`](./03_Terminal_Based/Terminal_Commands_Summary.md) (Terminal Commands Cheat Sheet)

Each practical includes the aim, procedure, observations, result, and important viva questions.

---

### Reference Links
- https://github.com/sanjay-6727/EHack.git
- https://chat.mistral.ai/work/ff484e5e-b7bd-4e03-8404-6637059b26a9
