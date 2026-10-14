# Ethical Hacking Laboratory — Final Practical Examination Manual
## Complete 17-Tool Quick-Revision Guide

**Course:** ICS1561 – Ethical Hacking Laboratory  
**Document Type:** Final Practical Examination Revision Sheet (All 17 Tools)  
**Primary References:** Official Lab Manuals (Weeks 2–9, Kali Linux Installation Guide, Reconnaissance Tools, Nmap, Nikto, FOCA, Maltego, Shodan, Nessus, Robtex, Armitage)

> [!IMPORTANT]
> **Laboratory Authorization & Safety Notice:**  
> All commands and testing procedures in this guide must be executed **strictly against authorized laboratory virtual machines or explicit instructor-approved targets**. The IP addresses `192.168.56.10` and `192.168.56.0/24` are concrete training examples to illustrate syntax. Always verify your own assigned VM IP address and network interface using `ip a` and `ip route` before executing active network scans.

---

## Table of Contents
- [Browser-Based & Web-Interface Tools](#browser-based--web-interface-tools)
  1. [Robtex](#1-robtex)
  2. [Shodan](#2-shodan)
  3. [Google Dorking](#3-google-dorking)
  4. [Nessus Essentials](#4-nessus-essentials)
- [Windows Tools](#windows-tools)
  5. [FOCA](#5-foca)
  6. [Maltego](#6-maltego)
  7. [Sysmon](#7-sysmon)
- [Kali Linux GUI Tools](#kali-linux-gui-tools)
  8. [Wireshark](#8-wireshark)
  9. [Armitage](#9-armitage)
- [Terminal Tools & Environment](#terminal-tools--environment)
  10. [Kali Linux VM](#10-kali-linux-vm)
  11. [WHOIS](#11-whois)
  12. [nslookup](#12-nslookup)
  13. [Nmap](#13-nmap)
  14. [theHarvester](#14-theharvester)
  15. [Netdiscover](#15-netdiscover)
  16. [Sublist3r](#16-sublist3r)
  17. [Nikto](#17-nikto)
- [One-Page Final Practical Examination Revision Table](#one-page-final-practical-examination-revision-table)
- [Core Theoretical Definitions & Record Writing Guidelines](#core-theoretical-definitions--record-writing-guidelines)

---

# Browser-Based & Web-Interface Tools

---

### 1. Robtex

**Aim**  
To perform passive domain footprinting, DNS analysis, and network intelligence gathering on a target domain without direct host interaction.

**Command / Procedure**  
*Website:* `https://www.robtex.com`
1. Open `https://www.robtex.com` in a web browser.
2. Enter the target domain `ssn.edu.in` in the search bar and press Enter.
3. Select the domain result to access the comprehensive DNS and network report.
4. Review the DNS record summary:
   - **A Record:** Maps the hostname directly to its 32-bit IPv4 address.
   - **MX Record:** Specifies the mail exchange servers responsible for handling domain email.
   - **NS Record:** Identifies the authoritative name servers managing the domain zone.
   - **TXT Record:** Stores text attributes such as SPF, DKIM, and domain verification strings.
   - **PTR Record:** Maps an IP address back to its canonical hostname (reverse DNS).
   - **SOA Record:** Identifies the primary master name server, zone administrator, and refresh timers.
5. Inspect the **DNS History** and **DNS Trace** graph to observe delegation paths and historical record modifications.
6. Examine the **IP Lookup** section to inspect the Autonomous System Number (ASN), BGP network route, and shared hosting neighbors.

**Expected Output**  
Resolved IPv4 addresses, authoritative nameservers, mail exchange priorities, graphical DNS resolution traces, and hosting ASN/network information.

**Observation**  
Robtex was used to perform passive DNS analysis and network footprinting for `ssn.edu.in`. In the recorded session, the primary A record resolved to `182.75.25.254`, and delegated nameservers were observed as `pdns.ssn.edu.in`, `sdns.ssn.edu.in`, and `tdns.ssn.edu.in`. The corresponding BGP routing prefix and Autonomous System Number were cataloged without generating direct network packets toward the target infrastructure.

**Result**  
Passive DNS analysis and infrastructure footprinting of the target domain were completed successfully.

---

### 2. Shodan

**Aim**  
To examine publicly observed internet-connected devices, open ports, protocol banners, and service configurations using an OSINT search engine.

**Command / Procedure**  
*Website:* `https://www.shodan.io`
1. Open `https://www.shodan.io` in a web browser.
2. Enter the target public IP address `8.8.8.8` (or an authorized query) into the search bar and press Enter.
3. Review the general host summary panel: IP address, ISP/organization (`Google LLC`), Autonomous System Number (`AS15169`), and physical geographic location.
4. Inspect the list of publicly observed open ports (e.g., port 53/DNS, port 443/HTTPS).
5. Examine individual service banners, SSL/TLS certificate metadata, supported protocol ciphers, and detected software versions.

**Expected Output**  
Indexed host profile displaying geographic location, hosting organization, listening ports, protocol banners, and TLS certificate details.

**Observation**  
Shodan was used to investigate publicly indexed information for target IP `8.8.8.8`. The host profile confirmed ownership by Google LLC under AS15169, with listening services identified on standard ports including DNS (port 53). Service banners and SSL certificate details were reviewed from pre-indexed scanner data without initiating active scans against the target.

**Result**  
Publicly indexed device intelligence, open services, and banner information were retrieved and analyzed.

---

### 3. Google Dorking

**Aim**  
To discover publicly indexed sensitive files, exposed directories, and specific documentation using advanced search engine operators.

**Command / Procedure**  
*Website:* `https://www.google.com`  
Execute the following complete query examples in the search box:
- `site:owasp.org`  
  *Restricts search results strictly to web pages hosted under the specified domain.*
- `filetype:pdf site:owasp.org cybersecurity`  
  *Restricts results to PDF documents containing the keyword "cybersecurity" on `owasp.org`.*
- `intitle:"cheat sheet" site:owasp.org`  
  *Finds pages whose HTML title tag contains the exact phrase "cheat sheet" on `owasp.org`.*
- `inurl:testing-guide site:owasp.org`  
  *Finds pages whose URL path contains the string "testing-guide" on `owasp.org`.*

*Additional Key Operators:*
- `intext:` restricts the search to terms appearing in the body text of the page.
- `-` (minus sign) excludes specific terms or domains from results (e.g., `-site:youtube.com`).
- `" "` (double quotes) forces exact-match phrase searching.
- `OR` searches for either of two conditions (must be entered in uppercase).

**Expected Output**  
Filtered search results containing targeted web pages, specific PDF documents, or URL paths matching the criteria without extraneous results.

**Observation**  
Google Dorking was performed using advanced search operators against `owasp.org`. Queries utilizing `site:`, `filetype:pdf`, `intitle:`, and `inurl:` successfully isolated specific technical documentation and guide files while eliminating irrelevant public web results.

**Result**  
Targeted public information and document links were successfully discovered using advanced search operators.

---

### 4. Nessus Essentials

**Aim**  
To perform an automated vulnerability assessment and configuration audit against an authorized target host.

**Command / Procedure**  
*Access URL:* `https://localhost:8834`
1. Open `https://localhost:8834` in a browser and sign in using your configured laboratory credentials.
2. Click **My Scans** $\rightarrow$ select **New Scan** $\rightarrow$ choose the **Basic Network Scan** template.
3. In the **Settings** tab:
   - **Name:** Enter `Lab-Vulnerability-Audit`.
   - **Folder:** Select `My Scans`.
   - **Targets:** Enter the confirmed authorized lab VM IP: `192.168.56.10`.
4. Click **Save**, then click the **Launch** button (play icon) to start scanning.
5. Wait for the status indicator to change from *Running* to *Completed*.
6. Click the completed scan to inspect the vulnerabilities dashboard:
   - **Critical:** Remotely exploitable vulnerabilities causing complete system compromise; requires immediate remediation.
   - **High:** Flaws allowing unauthorized access, code execution, or privilege escalation.
   - **Medium:** Configuration issues or flaws facilitating sensitive data disclosure or denial of service.
   - **Low:** Minor configuration flaws or non-critical information leaks.
   - **Info:** Informational enumeration items (open ports, service banners) with no direct vulnerability.
7. Click specific findings to review plugin details, CVE numbers, CVSS scores, and remediation advice, then click **Export** $\rightarrow$ **PDF/HTML** to save the report.

**Expected Output**  
Interactive vulnerability dashboard with colored severity bars, categorized vulnerability list, CVE references, and remediation guidance.

**Observation**  
Nessus Essentials was used to perform an automated vulnerability audit against authorized lab target `192.168.56.10`. The scan identified open services, evaluated host configurations against modular vulnerability plugins, and classified findings across severity levels (Critical, High, Medium, Low, Info) with corresponding CVE identifiers and vendor remediation steps.

**Result**  
The vulnerability assessment scan was executed and the security findings were reviewed and documented.

---

# Windows Tools

---

### 5. FOCA

**Aim**  
To discover publicly available documents on a target domain and extract embedded metadata to reveal internal organizational details.

**Command / Procedure**  
*Platform:* Windows Desktop Application
1. Launch FOCA and navigate to **Project** $\rightarrow$ **New Project**.
2. Enter the project name `FOCA-Lab` and set the target domain to `owasp.org`.
3. Under **Extensions**, select target file types: `.pdf`, `.doc`, `.docx`, `.xls`, `.xlsx`, `.ppt`.
4. Select search engines (Google, Bing, DuckDuckGo) and click **Search All**.
5. Select discovered document links in the results pane, right-click, and select **Download All**.
6. Once downloaded, right-click the files and select **Extract All Metadata**.
7. In the left navigation tree, inspect extracted metadata categories: **Users**, **Folders**, **Software**, **Printers**, and **Operating Systems**.
*(Note: If search engines block automated queries with CAPTCHAs or network errors occur, verify downloaded files rather than assuming metadata extraction succeeded).*

**Expected Output**  
List of discovered public documents and extracted metadata fields displaying document authors/usernames, internal folder paths, software versions, and printer names.

**Observation**  
FOCA was used to discover public documents and extract embedded metadata for `owasp.org`. Publicly hosted document URLs were retrieved, downloaded, and analyzed, identifying internal document creation properties and software versions without sending direct network attacks to the target server.

**Result**  
Document discovery and metadata extraction were conducted and internal metadata properties were reviewed.

---

### 6. Maltego

**Aim**  
To visually map and analyze relationships between domain entities, nameservers, mail exchangers, IP addresses, and email addresses.

**Command / Procedure**  
*Platform:* Windows Desktop Application
1. Open Maltego on Windows and create a new blank graph (**Ctrl+N**).
2. From the **Entity Palette**, drag a **Domain** entity onto the canvas and set its property to `ssn.edu.in`.
3. Right-click the domain entity, select **All Transforms**, and run **To DNS Name - NS [Name Server]** and **To DNS Name - MX [Mail Server]**.
4. Right-click the domain entity and execute an email discovery transform: **To Email addresses [From Search Engines]**.
5. Select a discovered DNS name entity (such as a mail exchanger) and execute **To IP Address [DNS]**.
6. Rearrange and review the link-analysis graph to observe structural connections:
   - **Entity:** A node representing an object (Domain, IP Address, DNS Name, Email).
   - **Transform:** A query script that interrogates OSINT sources to discover linked entities.
   - **Graph:** The visual canvas representing relationships between entities via directional links.

**Expected Output**  
An interconnected visual graph displaying the central target domain linked to discovered nameservers, mail servers, IP addresses, and email nodes.

**Observation**  
Maltego was used to visually map the external infrastructure of `ssn.edu.in`. Automated transforms extracted authoritative nameservers, mail exchange hosts, and associated IP addresses, displaying them on a link-analysis graph that clearly revealed the structural relationships among the organization's public network assets.

**Result**  
OSINT entity relationships for the target domain were mapped and visually analyzed.

---

### 7. Sysmon (System Monitor)

**Aim**  
To install, configure, and inspect Windows host-level telemetry and security event logs for system and process monitoring.

**Command / Procedure**  
*Platform:* Windows (PowerShell Administrator)  
Execute the following commands in an elevated PowerShell window:
```powershell
New-Item -ItemType Directory -Path C:\sys -Force
# Creates the working directory C:\sys if it does not already exist
Set-Location C:\sys
# Sets the active working directory to C:\sys
git clone https://github.com/SwiftOnSecurity/sysmon-config.git
# Clones the SwiftOnSecurity Sysmon configuration repository from GitHub
C:\Sysmon\Sysmon64.exe -c C:\sys\sysmon-config\sysmonconfig-export.xml
# Updates the configuration of the installed Sysmon64 service using the exported XML schema
```
*(Note: Ensure Git is installed before cloning. The `-c` flag updates an existing installation; fresh installation requires `-i`).*

*GUI Event Inspection Path:*
1. Press **Win + R**, type `eventvwr.msc`, and press Enter.
2. Navigate to:  
   `Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`
3. Click **Operational** to view captured security telemetry:
   - **Event ID 1:** Process Creation (captures Image path, ProcessId, CommandLine, ParentCommandLine, Hashes).
   - **Event ID 3:** Network Connection (captures Source/Destination IP, Port, Protocol, Image).

**Expected Output**  
PowerShell message confirming configuration update, followed by detailed process and network event entries in the Event Viewer Sysmon Operational log.

**Observation**  
Sysmon was configured on Windows using SwiftOnSecurity's XML configuration schema in PowerShell. The Event Viewer Sysmon Operational log was inspected, confirming real-time telemetry capture including Process Creation (Event ID 1) containing command-line arguments and parent process images.

**Result**  
Windows system activity monitoring and event log inspection were successfully configured and verified.

---

# Kali Linux GUI Tools

---

### 8. Wireshark

**Aim**  
To capture live network packets, analyze protocol headers across OSI layers, and apply display filters to isolate specific network traffic.

**Command / Procedure**  
*Platform:* Kali Linux GUI
1. Launch Wireshark from the Kali application menu or by running `sudo wireshark` in terminal.
2. Double-click the active network interface (e.g., `eth0`) to begin packet capture.
3. Open a terminal and generate ICMP test traffic:
   ```bash
   ping -c 4 1.1.1.1
   # Sends 4 ICMP echo request packets to Cloudflare DNS
   ```
4. Return to Wireshark and click the red square **Stop** button.
5. In the display filter bar, apply protocol filters:
   - `icmp` — filters only ICMP echo requests and replies.
   - `tcp` — filters TCP segments.
   - `udp` — filters UDP datagrams.
   - `dns` — filters DNS query and response packets.
6. Select an ICMP packet and expand the packet decode tree:
   - **Ethernet II:** Layer 2 Source/Destination MAC addresses.
   - **Internet Protocol Version 4:** Layer 3 Source/Destination IP addresses.
   - **Internet Control Message Protocol:** Type 8 (Echo Request) / Type 0 (Echo Reply).  
   *(Important: ICMP operates at Layer 3; it does not possess TCP or UDP port numbers).*
7. Save the capture via **File** $\rightarrow$ **Save As...** as `wireshark-practice.pcapng`.

**Expected Output**  
Packet list displaying packet numbers, timestamps, source/destination IPs, protocols, lengths, and decoded protocol header trees for each layer.

**Observation**  
Wireshark was used to capture and inspect live network traffic generated by ICMP echo requests. Packet headers were examined across the Data Link, Network, and Transport layers, display filters were applied to isolate ICMP frames, and the capture was exported to `wireshark-practice.pcapng`.

**Result**  
Network packet capture, protocol header analysis, and display filtering were verified successfully.

---

### 9. Armitage

**Aim**  
To manage host discovery, network port scanning, and exploit module correlation through a graphical front-end for the Metasploit Framework.

**Command / Procedure**  
*Platform:* Kali Linux GUI  
*Service Startup Commands:*
```bash
sudo msfdb status    # Checks whether the Metasploit PostgreSQL database service is running
sudo msfdb start     # Starts the PostgreSQL database service required by Metasploit and Armitage
sudo armitage        # Launches the Armitage graphical user interface
```
*GUI Workflow:*
1. Launch Armitage and click **Connect** on the RPC connection dialog.
2. Confirm the active network route in terminal using `ip route`.
3. In Armitage, choose **Hosts** $\rightarrow$ **Nmap Scan** $\rightarrow$ **Quick Scan**.
4. Enter the authorized lab subnet or host IP: `192.168.56.0/24` (or `192.168.56.10`).
5. Discovered targets appear on the central canvas as computer icons; right-click an icon $\rightarrow$ **Services** to view open ports (e.g., 22/SSH, 80/HTTP).
6. Click **Attacks** $\rightarrow$ **Find Attacks** to let Armitage correlate detected open ports against available Metasploit exploit modules.
7. Right-click the target host $\rightarrow$ select **Attack...** to inspect suggested attack categories.  
*(Note: Suggested attack modules represent theoretical matches based on open ports; they do not prove exploitability without verification).*

**Expected Output**  
Target host computer icons displayed on the canvas, detected services listed in the host view, and populated Attack context menus.

**Observation**  
Armitage was launched and connected to the Metasploit database. A Quick Scan across authorized lab subnet `192.168.56.0/24` mapped active hosts and open services to graphical canvas icons, and the Find Attacks utility was executed to identify potentially matching exploit modules.

**Result**  
Host discovery and automated attack-module correlation were executed through the Armitage graphical interface.

---

# Terminal Tools & Environment

---

### 10. Kali Linux VM

**Aim**  
To configure, verify, and operate the Kali Linux virtual machine environment for ethical hacking laboratory exercises.

**Command / Procedure**  
*Platform:* Kali Linux Terminal / Oracle VirtualBox  
*(Note: Kali Linux is an operating system installed via VirtualBox and ISO, not an apt package).*  
Execute these verification commands:
```bash
cat /etc/os-release    # Displays operating system distribution, release version, and build name
ip a                   # Lists network interfaces, MAC addresses, and assigned IP addresses
ip route               # Displays the kernel routing table, local subnet CIDR, and default gateway
ping -c 4 1.1.1.1      # Transmits four ICMP echo request packets to verify outbound connectivity
```

**Expected Output**  
Operating system release information, active network adapter details (e.g., `eth0`), valid IP address, default gateway route, and ICMP echo responses.

**Observation**  
The Kali Linux virtual machine was started and verified in VirtualBox. Terminal commands confirmed the Debian-based distribution details, active network adapter configuration, assigned IP address, and default gateway connectivity.

**Result**  
The Kali Linux security testing environment was verified and confirmed ready for laboratory practicals.

---

### 11. WHOIS

**Aim**  
To query public domain registrars and Regional Internet Registries to retrieve domain ownership, registration dates, and nameserver information.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update          # Refreshes local package repository metadata
sudo apt install whois   # Installs the WHOIS command-line query client
```
*Commands:*
```bash
whois --version                                # Displays installed WHOIS client version
whois --help                                   # Displays command help manual and available flags
whois example.com                              # Retrieves registration details for the example domain
whois 8.8.8.8                                  # Retrieves ASN, network CIDR range, and ISP details for the IP
whois -h whois.verisign-grs.com example.com    # Queries the authoritative Verisign registry server directly (-h)
```

**Expected Output**  
Domain registrar name, creation and expiration dates, domain status codes, authoritative nameservers, and IP network allocation data (with personal registrant details often marked as redacted).

**Observation**  
WHOIS was used to retrieve public registration records for domain `example.com` and IP `8.8.8.8`. The output was examined to identify the registrar, registration timeline, authoritative nameservers, and network CIDR blocks, noting that registrant contact details were protected under privacy redactions.

**Result**  
Public domain and IP registration information was successfully queried and reviewed.

---

### 12. nslookup

**Aim**  
To query Domain Name System (DNS) servers to resolve hostnames to IP addresses and examine specific DNS resource records.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update              # Refreshes local package repository metadata
sudo apt install dnsutils    # Installs BIND DNS utilities package containing nslookup and dig
```
*(Note: On newer Debian/Kali releases, use `sudo apt install bind9-dnsutils` if `dnsutils` is deprecated).*

*Commands:*
```bash
nslookup example.com               # Performs forward DNS lookup, resolving domain to IPv4/IPv6 addresses
nslookup -type=mx example.com      # Queries Mail Exchanger records (-type=mx) for email handling servers
nslookup -type=ns example.com      # Queries Name Server records (-type=ns) for authoritative zone servers
nslookup example.com 8.8.8.8       # Queries domain resolution explicitly using Google Public DNS (8.8.8.8)
nslookup 8.8.8.8                   # Performs reverse DNS lookup using PTR records to map IP back to hostname
```

**Expected Output**  
Forward A/AAAA record IP addresses, mail server hostnames with preference priorities, authoritative nameservers, and reverse DNS PTR hostname mappings.

**Observation**  
`nslookup` was used to resolve domain `example.com` and query DNS resource records. Forward queries resolved domain IP addresses, `-type=mx` identified responsible mail exchange servers, `-type=ns` determined authoritative nameservers, and a reverse query on `8.8.8.8` confirmed PTR resolution to `dns.google`.

**Result**  
DNS forward resolution, reverse resolution, and record-specific queries were completed successfully.

---

### 13. Nmap

**Aim**  
To perform active network host discovery, port scanning, service version detection, and operating system fingerprinting against an authorized target.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update                # Refreshes local package repository metadata
sudo apt install nmap -y       # Installs Nmap network scanner, auto-confirming prompts (-y)
```
*Commands:*
```bash
nmap --version                          # Displays installed Nmap version and feature flags
nmap -sn 192.168.56.0/24                # Host discovery only (ping scan); disables port scanning (-sn)
nmap 192.168.56.10                      # Default port scan checking top 1,000 common TCP ports
nmap -p 22,80,443,587 192.168.56.10     # Restricts scan strictly to specified comma-separated ports (-p)
nmap -p- 192.168.56.10                  # Scans all 65,535 TCP ports on the target (-p-)
sudo nmap -sS 192.168.56.10             # Executes stealthy TCP SYN (half-open) scan using raw sockets (-sS)
nmap -sV 192.168.56.10                  # Probes open ports to detect service names and versions (-sV)
sudo nmap -O 192.168.56.10              # Probes TCP/IP stack implementation for OS detection (-O)
sudo nmap -sS -sV -O 192.168.56.10      # Combines SYN scan, service versioning, and OS fingerprinting
sudo nmap -A 192.168.56.10              # Aggressive scan: combines OS detection, versioning, scripts, traceroute
```
*(Manual Correction: To scan selected ports use `-p 22,80,443,587`; the manual's `-p- 22,80,443,587` is invalid. `-p-` scans all ports).*  
*(Safety: `192.168.56.10` and `192.168.56.0/24` are training examples; verify your actual target IP and subnet with `ip a` and `ip route`).*

**Expected Output**  
Host status (up/down), port states (open, closed, filtered), listening service names, detected software versions, and operating system classification.

**Observation**  
Nmap was used to perform host discovery and port scanning on authorized lab target `192.168.56.10`. Host discovery confirmed target availability, port scanning identified open TCP ports, service detection (`-sV`) fingerprinted listening application versions, and OS detection (`-O`) identified the underlying operating system.

**Result**  
Network discovery, port scanning, service banner detection, and OS fingerprinting were performed successfully.

---

### 14. theHarvester

**Aim**  
To perform passive OSINT reconnaissance and harvest publicly available emails, subdomains, hostnames, and IP addresses associated with a domain.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update                   # Refreshes local package repository metadata
sudo apt install theharvester     # Installs theHarvester reconnaissance utility
```
*Commands:*
```bash
theHarvester -h                                         # Displays help manual and supported OSINT search backends (-h)
theHarvester -d microsoft.com -b crtsh                  # Queries crt.sh Certificate Transparency logs for subdomains (-b crtsh)
theHarvester -d example.com -b google                   # Searches Google index for publicly listed domain emails and hosts
theHarvester -d example.com -b crtsh,hackertarget -l 20 # Queries multiple sources simultaneously, limiting results to 20 (-l 20)
```
*(Note: Public search engines may block queries with CAPTCHAs or HTTP 429 rate limits; Certificate Transparency logs `-b crtsh` provide reliable results without API keys).*

**Expected Output**  
Lists of discovered public subdomains, virtual hosts, and indexed email addresses associated with the target domain.

**Observation**  
theHarvester was used to collect public OSINT for domain `microsoft.com`. Querying Certificate Transparency logs via `-b crtsh` retrieved active registered subdomains and hostnames without transmitting direct network packets to the target organization's servers.

**Result**  
Passive OSINT collection of domain subdomains and public host records was completed successfully.

---

### 15. Netdiscover

**Aim**  
To discover live hosts and identify IP and MAC addresses on a local Ethernet or wireless network segment using active and passive ARP requests.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update                 # Refreshes local package repository metadata
sudo apt install netdiscover    # Installs the Netdiscover Layer 2 ARP scanner
```
*Commands:*
```bash
netdiscover -h                                # Displays command usage syntax and operational flags (-h)
sudo netdiscover                              # Scans local network actively using the default network interface
sudo netdiscover -r 192.168.56.0/24           # Broadcasts sequential ARP requests across specified CIDR subnet (-r)
sudo netdiscover -i eth0                      # Binds ARP discovery process explicitly to interface eth0 (-i)
sudo netdiscover -p                           # Enables passive mode: sniffs ARP frames silently without sending packets (-p)
```
*(Safety: Confirm your actual interface and subnet using `ip a` and `ip route` before scanning; do not scan unknown or public networks).*

**Expected Output**  
Table of detected live hosts displaying IP address, MAC address, packet count, and hardware NIC vendor identification.

**Observation**  
Netdiscover was executed on authorized lab subnet `192.168.56.0/24` using interface `eth0`. Sequential ARP requests elicited replies from active virtual machines, revealing their assigned IP addresses, MAC addresses, and hardware NIC vendor classifications.

**Result**  
Layer 2 ARP host discovery was completed and local active devices were identified.

---

### 16. Sublist3r

**Aim**  
To discover publicly listed website subdomains using search engine scraping and threat intelligence platforms.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update                 # Refreshes local package repository metadata
sudo apt install sublist3r -y   # Installs Sublist3r, automatically confirming prompts (-y)
```
*Commands:*
```bash
sublist3r --help                              # Displays command options, syntax, and engine selection flags
sublist3r -d microsoft.com                    # Enumerates subdomains for target domain across default search engines (-d)
sublist3r -d microsoft.com -o results.txt     # Discovers subdomains and exports full output list to results.txt (-o)
cat results.txt                               # Displays the saved list of discovered subdomains in terminal
```
*(Note: Sublist3r relies on public search engine scraping. If search engines block scrapers, isolate functional engines with `-e bing,yahoo` or use `theHarvester -b crtsh`).*

**Expected Output**  
List of unique subdomains harvested across supported search engines, and a formatted text file containing the saved findings.

**Observation**  
Sublist3r was used to enumerate publicly indexed subdomains for `microsoft.com`. Multiple search engines and public threat databases were scraped to identify active subdomains, and the discovered host list was successfully exported to `results.txt`.

**Result**  
Public subdomain enumeration was performed and results were exported and reviewed.

---

### 17. Nikto

**Aim**  
To perform an automated web server security assessment to identify outdated software, server misconfigurations, dangerous files, and missing security headers.

**Command / Procedure**  
*Installation:*
```bash
sudo apt update              # Refreshes local package repository metadata
sudo apt install nikto -y    # Installs Nikto web server scanner, auto-confirming prompts (-y)
```
*Commands:*
```bash
nikto -Version                                         # Displays installed Nikto engine and plugin database release
nikto -h http://192.168.56.10                          # Audits HTTP web server on example target IP (-h)
nikto -h 192.168.56.10 -p 443 -ssl                     # Audits HTTPS web server over SSL/TLS connection on port 443 (-p, -ssl)
nikto -h http://192.168.56.10 -o nikto-report.txt     # Scans target and exports full textual audit findings to file (-o)
```
*(Safety: Nikto is noisy and active, sending thousands of HTTP requests. Scan only authorized lab VMs such as DVWA or Metasploitable. Replace `192.168.56.10` with your verified lab web target).*

**Expected Output**  
Web server banner disclosure, outdated software warnings, accessible administrative paths or default files, and missing defensive HTTP security headers.

**Observation**  
Nikto was used to perform a web server configuration assessment against authorized lab target `http://192.168.56.10`. The scan identified the running web server version, flagged missing security headers (`X-Frame-Options`, `X-Content-Type-Options`), located accessible directories, and exported findings to `nikto-report.txt`.

**Result**  
Web server configuration and vulnerability scanning were completed and findings were documented.

---

# One-Page Final Practical Examination Revision Table

| # | Tool | Platform | Main Purpose | Main Command or GUI Action | Nature |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | **Robtex** | Browser | Passive domain footprinting & DNS analysis | Search `ssn.edu.in` at `https://www.robtex.com` | Passive |
| **2** | **Shodan** | Browser | OSINT search for internet-connected devices | Search `8.8.8.8` at `https://www.shodan.io` | Passive |
| **3** | **Google Dorking** | Browser | Advanced search operator intelligence gathering | `site:owasp.org filetype:pdf cybersecurity` | Passive |
| **4** | **Nessus Essentials** | Web GUI | Automated host vulnerability assessment | New Scan $\rightarrow$ Basic Network Scan on `192.168.56.10` | Active |
| **5** | **FOCA** | Windows GUI | Public document discovery & metadata extraction | Project $\rightarrow$ `owasp.org` $\rightarrow$ Search All $\rightarrow$ Extract Metadata | Passive |
| **6** | **Maltego** | Windows GUI | Visual entity relationship link analysis | Drag Domain $\rightarrow$ Run Transforms (DNS, MX, Email, IP) | Passive |
| **7** | **Sysmon** | Windows CLI/GUI | Host system telemetry & security event logging | `Sysmon64.exe -c sysmonconfig.xml` & Event Viewer | Local Host |
| **8** | **Wireshark** | Kali GUI | Live packet capture & OSI layer protocol decode | Select interface $\rightarrow$ Capture $\rightarrow$ Filter `icmp` | Sniffer |
| **9** | **Armitage** | Kali GUI | Metasploit GUI for host scans & attack matching | `Hosts → Nmap Scan` & `Attacks → Find Attacks` | Active |
| **10** | **Kali Linux VM** | VirtualBox/CLI | Security testing OS verification & setup | `cat /etc/os-release`, `ip a`, `ip route` | Local Host |
| **11** | **WHOIS** | Kali Terminal | Public domain registrar & IP ownership lookup | `whois example.com` / `whois 8.8.8.8` | Passive |
| **12** | **nslookup** | Kali Terminal | DNS forward, reverse, and record query resolution | `nslookup -type=mx example.com` | Network DNS |
| **13** | **Nmap** | Kali Terminal | Host discovery, port scanning, OS/service detection | `sudo nmap -sS -sV -O 192.168.56.10` | Active |
| **14** | **theHarvester** | Kali Terminal | OSINT collection of domain subdomains & emails | `theHarvester -d microsoft.com -b crtsh` | Passive |
| **15** | **Netdiscover** | Kali Terminal | Layer 2 ARP local host discovery & MAC resolution | `sudo netdiscover -r 192.168.56.0/24` | Active/Passive |
| **16** | **Sublist3r** | Kali Terminal | Search engine scraping for subdomain discovery | `sublist3r -d microsoft.com -o results.txt` | Passive |
| **17** | **Nikto** | Kali Terminal | Web server vulnerability & configuration scanner | `nikto -h http://192.168.56.10` | Active |

---

# Core Theoretical Definitions & Record Writing Guidelines

### 1. Essential Viva Definitions (One Line Each)
- **Reconnaissance:** The initial phase of ethical hacking focused on systematically gathering information and intelligence about a target organization or system.
- **Passive vs. Active Reconnaissance:** Passive reconnaissance collects publicly available intelligence without directly interacting with target systems (undetectable), whereas active reconnaissance transmits packets directly to the target (detectable by logs and security controls).
- **DNS Records (A, MX, NS):**
  - **A Record:** Maps a human-readable hostname directly to a 32-bit IPv4 address.
  - **MX Record:** Specifies the mail exchange server responsible for accepting domain email and its delivery priority.
  - **NS Record:** Designates the authoritative name server holding the official DNS zone records for the domain.
- **IP Address vs. MAC Address:** An IP address is a logical Layer 3 network identifier used for routing across interconnected networks, whereas a MAC address is a permanent physical Layer 2 hardware address used within a local network segment.
- **Open Port vs. Vulnerability:** An open port is a network socket actively listening for incoming network traffic; a vulnerability is a specific defect, flaw, or misconfiguration in software that an attacker can exploit to violate security policy.
- **Bridged Adapter vs. NAT Mode:** NAT mode places the virtual machine behind the host's private virtual router with outbound-only access by default, whereas Bridged Adapter connects the VM directly to the physical network as an independent device with its own LAN IP.
- **CVE & Vulnerability Severity:** Common Vulnerabilities and Exposures (CVE) is a standardized dictionary of publicly known security flaws, ranked by Common Vulnerability Scoring System (CVSS) severity from Low to Critical based on exploitability and impact.

### 2. What to Write in Your Practical Record
- **Aim:** Exactly one clear sentence stating the objective of the practical exercise.
- **Procedure:** Step-by-step numbered instructions or exact commands executed in the session.
- **Observation:** Two or three concise sentences describing the actual output, findings, or interface results observed (without fabricating values).
- **Result:** Exactly one concluding sentence confirming that the procedure was performed and verified.
