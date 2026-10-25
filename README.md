# Ethical Hacking Lab Tools

# ⚡ Ethical Hacking Lab Quick-Reference Manual (ICS1561)

> **GitHub README tip:** Copy commands from the fenced code blocks. Replace example domains/IPs with targets you own or are explicitly authorized to test. Commands are written as plain text for easy copy/paste.

---

## 🌐 01. Web & OSINT Tools (Browser-Based & Recon)

### 🔍 Google Dorking

- **Basic Search:** Search for indexed pages on a domain.
  ```text
  site:microsoft.com
  ```
- **Target Directories / Pages:**
  ```text
  site:microsoft.com careers
  ```
- **File Harvesting (PDFs):**
  ```text
  site:microsoft.com filetype:pdf
  ```
- **Targeting Strings:**
  ```text
  site:microsoft.com "contact us"
  ```
- **Search for potentially exposed terms:** Use only for authorized security research; verify findings responsibly.
  ```text
  site:example.com inurl:"confidential"
  ```

### 🎯 Shodan.io (IoT & Banner Grabbing)

Open [Shodan](https://www.shodan.io/) in a browser and enter one of the following queries:

- **Search for a service/version banner:**
  ```text
  "vsftpd 2.3.4"
  ```
- **Port and country filter:**
  ```text
  port:21 country:"IN"
  ```
- **Organization filter:**
  ```text
  org:"Amazon.com" port:80
  ```

> These queries search Shodan's indexed results; they do not themselves authorize you to access or test discovered systems.

### 🌐 Network Recon Web Tools

#### Robtex
1. Open [robtex.com](https://www.robtex.com/).
2. Enter a domain you are permitted to research, for example:
   ```text
   ssn.edu.in
   ```
3. Review available DNS, IP, and infrastructure relationships.

#### Nessus Essentials
1. Open the Nessus web interface and sign in with the credentials configured during installation.
2. Select **New Scan**.
3. Choose an appropriate scan template.
4. Set the target to the IP address of your own lab VM (for example, Metasploitable).
5. Launch the scan and review the findings.

> Do not assume `login` / `login` are valid default credentials. Use the credentials you configured.

---

## 🪟 02. Windows Desktop & Kali GUI Tools

### 🛡️ Windows Systems — Sysmon Setup

Open **PowerShell or Command Prompt as Administrator**, change to the folder containing `Sysmon64.exe` and your XML config, then run:

```powershell
.\Sysmon64.exe -accepteula -i .\sysmonconfig-export.xml
```

Check Sysmon service status in PowerShell:

```powershell
Get-Service Sysmon*
```

> If the executable or configuration file has a different name or location, update the command accordingly.

### 🖼️ GUI OSINT & Analysis

#### FOCA
1. Open FOCA.
2. Create a project for a domain you are authorized to examine, for example:
   ```text
   example.com
   ```
3. Select the available search engines/sources and search for documents.
4. Download permitted public documents.
5. Right-click the project/results and select **Extract Metadata** (menu wording may vary by version).

#### Maltego
1. Open Maltego and create a new graph.
2. Drag an appropriate entity (such as **Domain** or **DNS Name**) onto the graph.
3. Enter a domain you are authorized to research:
   ```text
   example.com
   ```
4. Right-click the entity and select relevant transforms. **Run All Transforms** may not be available in every version or account tier.

### 📡 Traffic Analysis — Wireshark

1. Open Wireshark.
2. Select the interface that carries your own lab traffic (often **Wi-Fi**).
3. Click **Start Capturing Packets** (the button may be shown as a shark-fin icon; it is not usually called “Start Scan”).
4. Generate ordinary traffic, such as opening your own test page in Brave.
5. Return to Wireshark and click **Stop**.
6. Use a display filter to focus on web traffic:

   ```text
   http
   ```

   For HTTPS/TLS traffic, try:

   ```text
   tls
   ```

7. Select a packet and inspect **Internet Protocol Version 4/6** in the packet details to view source and destination IP addresses.

> Most modern YouTube/Brave traffic is HTTPS or QUIC, so you may not see readable webpage content. Capture only traffic you are authorized to monitor.

### 🧨 Armitage — Launch and Connect to a Local Lab

Run these commands in a **Kali Linux terminal**. They start PostgreSQL, initialize Metasploit's database if needed, and launch Armitage:

```bash
sudo service postgresql start
sudo msfdb init
sudo armitage
```

**GUI steps:**
1. Accept the connection defaults only if they match your local Metasploit setup.
2. Open **Hosts → Add Hosts** and enter the IP address of your isolated lab target, such as your own Metasploitable VM.
3. Use **Hosts → Nmap Scan → Quick Scan** if available in your version.
4. Review discovered hosts and services.
5. Only test exploits against an intentionally vulnerable lab VM that you own or have explicit permission to assess.

---

## 💻 03. Terminal-Based Tools (Kali Linux)

### 🔍 Passive & Active Footprinting

#### WHOIS

```bash
whois example.com
```

#### DNS lookup — MX records

```bash
nslookup -type=MX example.com
```

#### DNS lookup — NS records

```bash
nslookup -type=NS example.com
```

> Replace `example.com` with a domain you are authorized to research. Public DNS queries are generally passive from the target's perspective, but the DNS resolver still receives the query.

#### Netdiscover — ARP discovery on your own lab subnet

First identify your lab subnet:

```bash
ip addr
ip route
```

Then scan only your authorized lab subnet. Example subnet:

```bash
sudo netdiscover -r 192.168.58.0/24
```

> Change `192.168.58.0/24` to the subnet assigned to your own lab network. ARP discovery is active traffic on the local network.

### 📋 Subdomain & OSINT Enumeration

#### theHarvester

```bash
theHarvester -d example.com -l 100 -b all
```

Alternative: use a specific supported source:

```bash
theHarvester -d example.com -b hackertarget
```

> Available data sources and accepted options depend on the installed theHarvester version. If `-b all` is unsupported, check `theHarvester -h`.

#### Subfinder

```bash
subfinder -d example.com
```

Save the results to a file:

```bash
subfinder -d example.com -o subdomains.txt
```

#### Sublist3r (if installed)

```bash
sublist3r -d example.com -t 10 -o subdomains.txt
```

If Subfinder is not installed, Kali/Debian package availability can vary. Check the package manager first:

```bash
apt search subfinder
```

If the package is available, install it:

```bash
sudo apt update
sudo apt install subfinder
```

### 🛠️ Vulnerability Scanning & Web Assessment

#### Start a local test web server

Run this in the directory you want to serve:

```bash
python3 -m http.server 8000
```

Keep that terminal open. In another terminal, test the server:

```bash
curl -I http://127.0.0.1:8000
```

#### Nikto web scan — local test server

```bash
nikto -h http://127.0.0.1:8000
```

> Python's built-in server is intentionally minimal and is not a production web server. Nikto may report limited or non-actionable findings against it; use a purpose-built training target for a fuller lab.

#### Nmap port scanning

**Quick scan of your own machine:**

```bash
nmap 127.0.0.1
```

**OS detection on your own machine (may require root and may be inconclusive on localhost):**

```bash
sudo nmap -O 127.0.0.1
```

**Service/version detection against your own lab VM:**

```bash
nmap -sV <LAB_TARGET_IP>
```

**Default scripts and version detection against your own lab VM:**

```bash
nmap -sV -sC <LAB_TARGET_IP> -oN scan-results.txt
```

**SYN scan, service detection, OS detection, and default scripts against your own lab VM:**

```bash
sudo nmap -sS -sV -O -sC <LAB_TARGET_IP> -oN temp.txt
```

Replace `<LAB_TARGET_IP>` with the actual IP address of your authorized lab machine. For example, if your isolated Metasploitable VM is `192.168.58.101`, use:

```bash
sudo nmap -sS -sV -O -sC 192.168.58.101 -oN temp.txt
```

> Avoid copying a public IP such as `8.8.8.8` into an aggressive scan command. Scan only systems you own or have explicit authorization to test.

---

## 📁 Repository Structure

1. **[`01_Browser_Based/`](./01_Browser_Based/)** — Web & OSINT tools
   - [`Browser_Tools.md`](./01_Browser_Based/Browser_Tools.md) — Robtex, Shodan, Google Dorking, Nessus Essentials
2. **[`02_Windows_and_GUI/`](./02_Windows_and_GUI/)** — Windows desktop & Kali GUI tools
   - [`Windows_Tools.md`](./02_Windows_and_GUI/01_Windows_Tools/Windows_Tools.md) — FOCA, Maltego, Sysmon
   - [`Other_GUI_Tools.md`](./02_Windows_and_GUI/02_Other_GUI_Tools/Other_GUI_Tools.md) — Wireshark, Armitage
3. **[`03_Terminal_Based/`](./03_Terminal_Based/)** — Terminal-based tools (Kali Linux)
   - [`Terminal_Tools.md`](./03_Terminal_Based/Terminal_Tools.md) — Full practical notes: Kali VM, WHOIS, nslookup, Nmap, theHarvester, Netdiscover, Sublist3r, Nikto
   - [`Terminal_Commands_Summary.md`](./03_Terminal_Based/Terminal_Commands_Summary.md) — Terminal commands cheat sheet

Each practical includes the **aim, procedure, observations, result, and important viva questions**.

---

## 📚 Reference Links

- [GitHub repository](https://github.com/sanjay-6727/EHack.git)
- [Mistral conversation reference](https://chat.mistral.ai/work/ff484e5e-b7bd-4e03-8404-6637059b26a9)
