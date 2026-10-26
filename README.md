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

```
SET 1 — Ethical Hacking Model Exam
Short, record-ready answers for all 3 questions from your PDF, pages 2–4.
Q1. FOCA + Search Digg
Aim: To perform passive information gathering and extract document metadata using FOCA and Search Digg.

Problem Statement: To identify publicly exposed document metadata and indexed resources of an authorized domain.

Procedure:
1. Open FOCA and create a new project.
2. Enter the approved domain.
3. Search for public PDF, DOCX and XLSX files.
4. Extract metadata like author, software and creation date.
5. Open Search Digg and search for publicly indexed resources.
6. Record URLs, document types and visible information.
7. Compare findings, take screenshots and suggest security measures.
Final Observation: Write based on the metadata and documents found.

Q2. Robtex + Nessus
Aim: To gather domain intelligence using Robtex and identify vulnerabilities using Nessus.

Problem Statement: To examine public DNS information and assess vulnerabilities on an authorized lab host.

Procedure:
1. Open Robtex and search for the approved domain.
2. Examine DNS records such as A, AAAA, MX, NS and TXT.
3. Record IP addresses and name servers.
4. Open Nessus and create a Basic Network Scan.
5. Enter the authorized lab IP address as the target.
6. Start the scan and wait for completion.
7. Record open ports, services and detected vulnerabilities.
8. Classify findings by severity and suggest remediation.
Final Observation: Write the DNS details and vulnerability severity levels shown in your output.

Q3. Wireshark + Armitage
Aim: To analyze network traffic using Wireshark and validate vulnerabilities using Armitage in an isolated lab.

Problem Statement: To inspect captured packets, identify services and verify vulnerabilities on an authorized vulnerable virtual machine.

Procedure:
1. Start Kali Linux and the vulnerable virtual machine on an isolated network.
2. Open Wireshark and select the correct network interface.
3. Start packet capture and generate permitted lab traffic.
4. Apply filters such as icmp, dns, tcp and http.
5. Record source IP, destination IP and protocol details.
6. Open Armitage and connect to Metasploit.
7. Identify the lab host, open ports and services.
8. Perform instructor-approved, non-destructive vulnerability validation.
9. Record results and recommend patches or firewall restrictions.
Final Observation: Write the protocols, services and vulnerability-validation results observed in your output.

SET 1 complete! Memorize the Aim, Problem Statement and Procedure. For the final observation, write only what your actual output shows.
















set 2













SET 2 — Ethical Hacking Model Exam
Short, record-ready answers for all 3 questions from your PDF, pages 6–8.
Q1. WHOIS + nsLookup + Sublist3r
Aim: To gather domain registration details, DNS records and discover subdomains using WHOIS, nsLookup and Sublist3r.

Problem Statement: To create a domain footprint and identify potential security risks from publicly exposed information.

Procedure:
1. Obtain the instructor-approved domain.
2. Execute whois example.com to view registration details.
3. Execute nslookup example.com to find the IP address.
4. Execute nslookup -type=NS example.com to find name servers.
5. Execute nslookup -type=MX example.com to find mail servers.
6. Execute sublist3r -d example.com to discover subdomains.
7. Record the results, take screenshots and suggest security measures.
Final Observation: Write the registration details, DNS records and subdomains found.

Q2. Robtex + Nmap
Aim: To collect domain intelligence using Robtex and identify open ports and services using Nmap.

Problem Statement: To analyze publicly available network information and identify exposed services on an authorized lab host.

Procedure:
1. Open Robtex and search for the approved domain.
2. Record DNS records, IP addresses and name servers.
3. Open the terminal and check connectivity to the authorized lab host.
4. Execute nmap <lab-IP> to scan ports.
5. Execute nmap -sV <lab-IP> to identify service versions.
6. If instructed, execute nmap -sC <lab-IP> to run default scripts.
7. Save results using nmap -sV -oN nmap_results.txt <lab-IP>.
8. Analyze exposed services and suggest security measures.
Final Observation: Write the open ports, services and version details shown in the scan.

Q3. Nessus + Armitage
Aim: To identify vulnerabilities using Nessus and perform controlled vulnerability validation using Armitage.

Problem Statement: To assess security weaknesses in an authorized vulnerable virtual machine and recommend corrective actions.

Procedure:
1. Start Kali Linux and the vulnerable virtual machine on an isolated lab network.
2. Open Nessus and create a Basic Network Scan.
3. Enter the authorized lab IP address and start the scan.
4. Record detected ports, services, vulnerabilities and severity levels.
5. Open Armitage and connect to Metasploit.
6. Discover or import the authorized lab host.
7. Examine its ports and services.
8. Perform instructor-approved, non-destructive vulnerability validation.
9. Compare findings and recommend patches or access restrictions.
Final Observation: Write the vulnerabilities detected, their severity and the validation results.

SET 2 complete! Say “Set 3” when you're ready.
















set 3













SET 3 — Ethical Hacking Model Exam
Short, record-ready answers for all 3 questions from your PDF, pages 10–12.
Q1. Maltego + Shodan + Google Dorking
Aim: To identify publicly exposed assets and gather information using Maltego, Shodan and Google Dorking.

Problem Statement: To analyze an organization's public attack surface and identify potential information-disclosure risks.

Procedure:
1. Obtain the instructor-approved domain.
2. Open Maltego and create a new graph.
3. Add the domain and perform available transformations.
4. Record related domains, IP addresses and DNS information.
5. Open Shodan and search using hostname:example.com.
6. Record exposed hosts, ports and service details.
7. Use Google Dorking with site:example.com filetype:pdf and site:example.com filetype:docx.
8. Record findings, take screenshots and suggest security measures.
Final Observation: Write the publicly exposed assets and documents found.

Q2. Nmap + Nessus
Aim: To identify network services using Nmap and assess vulnerabilities using Nessus.

Problem Statement: To discover exposed ports and identify security weaknesses in an authorized laboratory server.

Procedure:
1. Obtain the authorized lab server IP.
2. Execute nmap <lab-IP> to identify open ports.
3. Execute nmap -sV <lab-IP> to identify service versions.
4. Save results using nmap -sV -oN service_report.txt <lab-IP>.
5. Open Nessus and create a Basic Network Scan.
6. Enter the authorized lab IP and start the scan.
7. Record vulnerabilities, severity levels and affected services.
8. Compare both outputs and recommend corrective measures.
Final Observation: Write the open ports, detected services and vulnerabilities found.

Q3. Wireshark + Armitage
Aim: To analyze network packets using Wireshark and validate vulnerabilities using Armitage.

Problem Statement: To investigate network communications and examine vulnerabilities on an authorized virtual machine.

Procedure:
1. Start Kali Linux and the vulnerable virtual machine on an isolated network.
2. Open Wireshark and select the correct network interface.
3. Start packet capture and generate permitted lab traffic.
4. Apply filters such as icmp, dns, tcp and http.
5. Record source IP, destination IP and protocol details.
6. Open Armitage and connect to Metasploit.
7. Identify the authorized host, open ports and services.
8. Perform instructor-approved, non-destructive vulnerability validation.
9. Record findings and suggest patches, traffic filtering or disabling unnecessary services.
Final Observation: Write the protocols observed and the vulnerability-validation results.

SET 3 complete! Say “Set 4” to continue.
















set 4













SET 4 — Ethical Hacking Model Exam
Short, record-ready answers for all 3 questions from your PDF, pages 15–18.
Q1. FOCA + theHarvester + Sysmon
Aim: To gather public information using FOCA and theHarvester and investigate security events using Sysmon.

Problem Statement: To identify information leakage and investigate suspicious activities on a Windows workstation.

Procedure:
1. Obtain the approved domain and Windows lab logs.
2. Open FOCA and create a new project.
3. Search for public documents and extract metadata.
4. Run theHarvester -d example.com -b bing to collect public information.
5. Record discovered email addresses and hostnames.
6. Open Windows Event Viewer.
7. Navigate to Applications and Services Logs → Microsoft → Windows → Sysmon → Operational.
8. Examine Event IDs 1 (process creation), 3 (network connection, if enabled) and 11 (file creation).
9. Record findings and recommend security measures.
Final Observation: Write the metadata, public information and relevant Sysmon events observed.

Q2. Robtex + Nessus
Aim: To collect domain and IP information using Robtex and identify vulnerabilities using Nessus.

Problem Statement: To assess the security of an authorized lab server and recommend corrective measures.

Procedure:
1. Obtain the approved domain and lab IP address.
2. Search for the domain in Robtex.
3. Record DNS records, IP addresses and name-server details.
4. Open Nessus and create a Basic Network Scan.
5. Enter the authorized lab IP address.
6. Start the scan and wait for completion.
7. Record vulnerability names, severity levels and affected services.
8. Analyze the findings and recommend patches, secure configurations or network restrictions.
Final Observation: Write the DNS information and vulnerabilities detected by Nessus.

Q3. Nmap + Armitage
Aim: To identify network services using Nmap and validate vulnerabilities using Armitage.

Problem Statement: To examine exposed ports and assess potential vulnerabilities on an authorized vulnerable virtual machine.

Procedure:
1. Start Kali Linux and the vulnerable virtual machine on an isolated network.
2. Execute nmap <lab-IP> to identify open ports.
3. Execute nmap -sV <lab-IP> to identify service versions.
4. If instructed, execute nmap -sC <lab-IP> to run default scripts.
5. Save results using nmap -sV -oN nmap_report.txt <lab-IP>.
6. Open Armitage and connect to Metasploit.
7. Discover the authorized lab host and examine its services.
8. Perform instructor-approved, non-destructive vulnerability validation.
9. Record findings and recommend patching or restricting unnecessary services.
Final Observation: Write the open ports, service versions and vulnerability-validation results.
```
kuppanatc
kuppared2006x

## 📚 Reference Links

- [GitHub repository](https://github.com/sanjay-6727/EHack.git)
- [Mistral conversation reference](https://chat.mistral.ai/work/ff484e5e-b7bd-4e03-8404-6637059b26a9)
