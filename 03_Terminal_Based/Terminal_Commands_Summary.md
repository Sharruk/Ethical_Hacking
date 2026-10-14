# Terminal Commands Summary — Ethical Hacking Laboratory

**Course:** ICS1561 – Ethical Hacking Laboratory  
**Category:** 03 – Terminal-Based Tools (Kali Linux)  
**Document Type:** Quick Revision Command Cheat Sheet & Flag Reference  
**Reference Manuals:** Kali Installation Guide, Nmap Manual, Reconnaissance Tools, Week 4 (Nikto & DNS), Week 2 Practical  
**Companion Detailed Notes:** [`Terminal_Tools.md`](./Terminal_Tools.md) | [`Terminal_Tools.docx`](./Terminal_Tools.docx)

---

## Table of Contents
1. [Important Safety Guidance & Concrete Training Examples](#1-important-safety-guidance--concrete-training-examples)
2. [Package Installation & Setup Reference](#2-package-installation--setup-reference)
3. [Kali Linux Network Verification & Diagnostic Commands](#3-kali-linux-network-verification--diagnostic-commands)
4. [WHOIS Command Table](#4-whois-command-table)
5. [nslookup Command Table & DNS Record Reference](#5-nslookup-command-table--dns-record-reference)
6. [Nmap Command Table & Port State Reference](#6-nmap-command-table--port-state-reference)
7. [theHarvester Command Table](#7-theharvester-command-table)
8. [Netdiscover Command Table](#8-netdiscover-command-table)
9. [Sublist3r Command Table](#9-sublist3r-command-table)
10. [Nikto Command Table & Finding Types](#10-nikto-command-table--finding-types)
11. [Master Command-Line Flag Reference Table](#11-master-command-line-flag-reference-table)
12. [One-Minute Practical Exam Revision Table](#12-one-minute-practical-exam-revision-table)

---

## 1. Important Safety Guidance & Concrete Training Examples

### Purpose of Concrete Examples
To make every command immediately actionable without guessing syntax, this summary uses concrete training parameters in place of abstract placeholders:

- **Example Lab Target IP:** `192.168.56.10` (designated target virtual machine, e.g., Metasploitable or DVWA).
- **Example Lab Subnet:** `192.168.56.0/24` (designated isolated Host-Only laboratory subnet).
- **Example Public Domain:** `example.com` (standard IANA reserved test domain for WHOIS and DNS).
- **Example OSINT Domain:** `microsoft.com` (domain used in the lab manuals for theHarvester and Sublist3r).
- **Example Public DNS Server:** `8.8.8.8` (Google Public DNS resolver).
- **Example Output Files:** `results.txt` (Sublist3r) and `nikto-report.txt` / `nikto-report.html` (Nikto).

> [!WARNING]
> **Laboratory Network Verification Notice:**  
> The IP addresses `192.168.56.10` and `192.168.56.0/24` are concrete examples illustrating complete command syntax. **They are not automatically your current VM settings.** In VirtualBox NAT mode, your subnet is typically `10.0.2.0/24` and your gateway is `10.0.2.2`. In Bridged mode, it matches your local physical router. Always verify your real interface and IP using `ip a` and `ip route` before running active scans. **Never scan unauthorized public networks or college campus networks.**

---

## 2. Package Installation & Setup Reference

### Package Manager Flags & Concepts
- `sudo` executes commands with root/superuser administrative privileges.
- `apt update` refreshes local package repository metadata from official Kali Linux mirrors.
- `apt install` downloads, unpacks, and configures the specified software packages and dependencies.
- `-y` automatically answers "yes" to all installation confirmation prompts, preventing terminal stalls.
- **Kali Linux VM Note:** Kali Linux is an entire operating system installed via Oracle VirtualBox and the official ISO file; it is **not** an apt package (there is no `sudo apt install kali`).
- **Pre-installed Verification:** Many tools (such as Nmap) are pre-installed in Kali. Run `command -v <tool>` to verify existence before attempting installation.

### Installation Commands in Execution Order

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `sudo apt update` | Standard package index refresh command without flags. | Synchronizes local package lists with the latest Kali Linux repository mirrors. |
| `sudo apt install whois` | Installs the WHOIS client utility package. | Installs the client used to query public domain and IP registration databases. |
| `sudo apt install dnsutils` | Installs the legacy BIND DNS utilities suite. | Provides essential DNS query binaries including `nslookup`, `dig`, and `host`. |
| `sudo apt install bind9-dnsutils` | Installs modern transitional package name if `dnsutils` is deprecated. | Provides `nslookup` on modern Debian/Kali releases if `dnsutils` cannot be located. |
| `sudo apt install nmap -y` | `-y` automatically confirms installation prompts. | Installs the Network Mapper active network scanner without manual prompt pauses. |
| `sudo apt install theharvester` | Installs the OSINT reconnaissance package. | Installs the Python-based intelligence gathering tool for emails, hosts, and subdomains. |
| `sudo apt install netdiscover` | Installs the Layer 2 network discovery package. | Installs the active and passive ARP network scanner for local Ethernet/wireless LANs. |
| `sudo apt install sublist3r -y` | `-y` automatically confirms installation prompts. | Installs the search engine scraping tool for website subdomain discovery. |
| `sudo apt install nikto -y` | `-y` automatically confirms installation prompts. | Installs the command-line web server vulnerability and configuration scanner. |

---

## 3. Kali Linux Network Verification & Diagnostic Commands

Run these diagnostic commands inside your Kali Linux terminal to determine your active network interface name (e.g., `eth0`, `eth1`), current IP address, and default gateway before executing network scans.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `cat /etc/os-release` | Reads the standardized Linux OS release metadata file. | Displays the installed Kali Linux distribution name, release version, and codename. |
| `ip a` | `a` is short for `addr` (lists address configurations). | Displays all active network interfaces, MAC addresses, and assigned IPv4/IPv6 addresses. |
| `ip route` | Lists kernel IP network routing table entries. | Displays the active network routes, local subnet CIDR, and default gateway IP address. |
| `ping -c 4 1.1.1.1` | `-c 4` limits transmission to exactly four ICMP echo packets. | Tests outbound Internet connectivity and packet transmission latency via Cloudflare DNS. |
| `command -v nmap` | `-v` returns the executable path or command name. | Verifies whether the `nmap` binary is installed and executable in the current `$PATH`. |

---

## 4. WHOIS Command Table

WHOIS is a TCP Port 43 query/response protocol used to inspect registration databases maintained by Regional Internet Registries (RIRs like ARIN, RIPE, APNIC) and ICANN domain registrars.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `whois --version` | `--version` outputs binary compilation details. | Displays the installed version and copyright information of the WHOIS utility. |
| `whois --help` | `--help` displays standard command-line help manual. | Shows all available operational parameters, query options, and syntax rules. |
| `whois example.com` | Standard query syntax querying default referral registries. | Retrieves registrar name, creation date, expiry date, and authoritative nameservers for `example.com`. |
| `whois 8.8.8.8` | Passes an IPv4 address to the WHOIS client. | Retrieves network CIDR range, Autonomous System Number (ASN), and ISP details for `8.8.8.8`. |
| `whois -h whois.verisign-grs.com example.com` | `-h` specifies the authoritative WHOIS server hostname. | Requests registration data for `example.com` directly from the designated Verisign registry server. |

### Output Types & Interpretation
- **Registrar:** The organization managing the domain reservation (e.g., GoDaddy, Namecheap).
- **Creation / Expiry Dates:** Dates indicating domain registration timeline and renewal status.
- **Name Servers:** Authoritative DNS servers delegated to resolve records for the zone.
- **Registrant Details:** Contact information, often marked as "REDACTED FOR PRIVACY" under GDPR regulations.

---

## 5. nslookup Command Table & DNS Record Reference

`nslookup` (Name Server Lookup) queries Domain Name System (DNS) servers on UDP/TCP Port 53 to resolve domain names to IP addresses and interrogate DNS resource records.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `nslookup` | Interactive mode started without command arguments. | Launches the interactive DNS lookup prompt for entering sequential query commands. |
| `nslookup example.com` | Standard forward DNS lookup using default resolver. | Resolves domain `example.com` to its associated IPv4 (A) or IPv6 (AAAA) addresses. |
| `nslookup -type=mx example.com` | `-type=mx` filters queries for Mail Exchanger records. | Identifies mail server hostnames and preference numbers handling incoming email for `example.com`. |
| `nslookup -type=ns example.com` | `-type=ns` filters queries for Name Server records. | Identifies authoritative DNS servers responsible for managing the `example.com` zone. |
| `nslookup example.com 8.8.8.8` | Explicitly queries through server `8.8.8.8` as second argument. | Resolves `example.com` directly using Google Public DNS instead of the local default resolver. |
| `nslookup 8.8.8.8` | Executes a reverse DNS lookup on an IPv4 address. | Queries reverse DNS PTR records to map IP address `8.8.8.8` back to `dns.google`. |

### DNS Resource Record Definitions
- **A Record (Address):** Maps a human-readable hostname directly to a 32-bit IPv4 address.
- **MX Record (Mail Exchanger):** Specifies mail servers responsible for receiving domain email and their delivery priority.
- **NS Record (Name Server):** Identifies authoritative DNS servers that hold the official zone records for the domain.
- **PTR Record (Pointer):** Resolves an IPv4 or IPv6 address back to its canonical hostname during reverse DNS lookups.

---

## 6. Nmap Command Table & Port State Reference

Nmap (Network Mapper) is an active network scanner that transmits crafted network packets to discover live hosts, identify open network ports, determine running services/versions, and fingerprint operating systems.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `nmap --version` | `--version` outputs binary version and compiled features. | Displays the installed Nmap version and active build features. |
| `nmap -h` | `-h` prints the comprehensive command-line help manual. | Displays available scanning techniques, timing options, and target specification flags. |
| `nmap -sn 192.168.56.0/24` | `-sn` disables port scanning (ping scan / host discovery). | Identifies active, responding live hosts across the example subnet `192.168.56.0/24`. |
| `nmap 192.168.56.10` | Standard default scan checks top 1,000 common TCP ports. | Determines port availability on target `192.168.56.10` using default TCP scanning. |
| `nmap -p 22,80,443,587 192.168.56.10` | `-p` restricts scanning strictly to listed port numbers. | Scans only TCP ports 22, 80, 443, and 587 on host `192.168.56.10`. |
| `nmap -p- 192.168.56.10` | `-p-` expands scanning range to all 65,535 TCP ports. | Scans every port (1–65535) on `192.168.56.10` to discover services on non-standard ports. |
| `sudo nmap -sS 192.168.56.10` | `-sS` performs a stealthy half-open TCP SYN scan (needs `sudo`). | Probes ports on `192.168.56.10` without completing 3-way handshakes (sends RST upon SYN/ACK). |
| `nmap -sV 192.168.56.10` | `-sV` probes open ports to interrogate application banners. | Detects application software names and version details running on `192.168.56.10`. |
| `sudo nmap -O 192.168.56.10` | `-O` analyzes TCP/IP stack implementation responses (needs `sudo`). | Attempts to identify the operating system brand and version of target `192.168.56.10`. |
| `sudo nmap -sS -sV -O 192.168.56.10` | Combines `-sS` (SYN), `-sV` (version), and `-O` (OS detection). | Performs stealth port scanning, service banner detection, and OS fingerprinting on `192.168.56.10`. |
| `sudo nmap -A 192.168.56.10` | `-A` activates aggressive scan (OS, version, scripts, traceroute). | Runs a comprehensive audit combining OS detection, versioning, default NSE scripts, and traceroute. |

### Lab Manual Syntax Correction Note
> [!IMPORTANT]
> **Lab Manual Typo Warning:**  
> The lab manual contains the erroneous command string `nmap -p- 22,80,443,587`.  
> - In Nmap syntax, `-p-` instructs the tool to scan **all 65,535 ports**.
> - To scan a specific list of ports, omit the trailing dash: `nmap -p 22,80,443,587 192.168.56.10`.
> - To scan all ports, use: `nmap -p- 192.168.56.10`.

### Nmap Port States
- **Open:** An application is actively accepting TCP connections or UDP packets on the port.
- **Closed:** The port receives packets and responds with a TCP RST, but no application is listening.
- **Filtered:** A firewall, packet filter, or network rule blocks probes, preventing Nmap from determining the state.

---

## 7. theHarvester Command Table

`theHarvester` is an Open Source Intelligence (OSINT) tool used during reconnaissance to collect publicly available emails, names, subdomains, IP addresses, and URLs from public search engines and databases without transmitting packets to the target server.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `theHarvester --version` | `--version` outputs binary release information. | Displays the installed software version of theHarvester utility. |
| `theHarvester -h` | `-h` displays command syntax and available data sources. | Shows usage instructions, option flags, and supported OSINT search engine backends. |
| `theHarvester -d example.com -b google` | `-d` specifies target domain, `-b google` selects Google. | Collects publicly indexed email addresses and hostnames for `example.com` via Google search. |
| `theHarvester -d example.com -b crtsh` | `-b crtsh` queries crt.sh Certificate Transparency logs. | Discovers active subdomains for `example.com` from public SSL/TLS certificate databases. |
| `theHarvester -d example.com -b crtsh,hackertarget -l 20` | `-b` specifies multiple sources, `-l 20` limits results to 20. | Queries crt.sh and HackerTarget simultaneously, returning up to 20 results for `example.com`. |
| `theHarvester -d microsoft.com -b all -l 300 -q` | `-b all` queries all backends, `-l 300` caps count, `-q` quiet. | Searches all supported sources for `microsoft.com` up to 300 items while suppressing banners. |

### Backend Availability Caution
> [!TIP]
> Public search engines like Google or Bing frequently block automated scraping tools with CAPTCHAs or HTTP 429 rate limits. For consistent lab practical results, use Certificate Transparency backends: `-b crtsh` or `-b certspotter`. Run `theHarvester -h` to verify active backends supported by your version.

---

## 8. Netdiscover Command Table

Netdiscover is an active and passive Layer 2 Address Resolution Protocol (ARP) reconnaissance tool designed to detect live hosts, IP addresses, and MAC addresses on local Ethernet and wireless networks.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `netdiscover -h` | `-h` displays command syntax and option switches. | Shows available operational switches, scanning modes, and usage syntax. |
| `netdiscover --version` | `--version` outputs compiled binary version. | Displays the installed Netdiscover release and build version. |
| `sudo netdiscover` | Runs active ARP discovery on default network interface (needs `sudo`). | Broadcasts ARP requests on default interface to discover local active devices. |
| `sudo netdiscover -r 192.168.56.0/24` | `-r` specifies the target CIDR network range. | Sends sequential ARP requests across subnet `192.168.56.0/24` to locate live hosts. |
| `sudo netdiscover -i eth0` | `-i` binds discovery to a specific interface card (`eth0`). | Executes ARP network discovery exclusively through interface `eth0`. |
| `sudo netdiscover -i eth0 -r 192.168.56.0/24` | `-i` binds interface, `-r` specifies target CIDR subnet. | Scans subnet `192.168.56.0/24` explicitly through the `eth0` network interface. |
| `sudo netdiscover -p` | `-p` enables passive listening mode (no transmitted packets). | Sniffs and analyzes unsolicited ARP broadcast traffic silently without sending packets. |
| `sudo netdiscover -S` | `-S` enables sleep between packet bursts (per lab manual). | Introduces delay between ARP request bursts to reduce network saturation (check `netdiscover -h`). |

### Local Network Scope
ARP operates strictly at Layer 2 (Data Link Layer) and does not traverse routers or the public internet. Netdiscover can only discover hosts on the same physical or virtual broadcast domain. Always verify your interface and subnet using `ip a` before scanning.

---

## 9. Sublist3r Command Table

Sublist3r is a Python-based OSINT subdomain enumeration utility that harvests subdomains from search engines (Google, Yahoo, Bing, Baidu, Ask) and platforms (Netcraft, VirusTotal, ThreatCrowd, DNSdumpster).

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `sublist3r --help` | `--help` displays command syntax and search parameters. | Shows supported options, engine selection flags, and output formatting rules. |
| `sublist3r -d microsoft.com` | `-d` designates the target domain for OSINT enumeration. | Queries default search engines to discover publicly exposed `microsoft.com` subdomains. |
| `sublist3r -d microsoft.com -o results.txt` | `-o` saves the discovered subdomains to a specified file. | Enumerates subdomains for `microsoft.com` and writes results into `results.txt`. |
| `cat results.txt` | Standard Linux utility to print file contents to terminal. | Displays the saved list of discovered subdomains in the terminal window. |

### Installation & Dependency Failure Note
> [!NOTE]
> Sublist3r is an older tool whose public search engine scrapers may encounter HTTP 429 rate limits or changed HTML structures. If search engines block queries, isolate functional engines with `-e bing,yahoo` or use `theHarvester -b crtsh` as an alternative.

---

## 10. Nikto Command Table & Finding Types

Nikto is an open-source command-line web server scanner that tests web servers for dangerous files, outdated software, misconfigurations, and missing security headers.

| Command | Option/flag explanation | One-line purpose |
| :--- | :--- | :--- |
| `nikto -Version` | `-Version` prints engine and plugin database release. | Displays the installed Nikto software version and plugin database update status. |
| `nikto -h http://192.168.56.10` | `-h` specifies the target web server host or base URL. | Performs a baseline web server vulnerability and configuration audit on `http://192.168.56.10`. |
| `nikto -h 192.168.56.10 -p 443 -ssl` | `-p 443` sets port, `-ssl` forces HTTPS encrypted connection. | Audits the secure HTTPS web server running on port 443 of `192.168.56.10`. |
| `nikto -h http://192.168.56.10 -o nikto-report.txt` | `-o` designates output report file (plain text format). | Scans `http://192.168.56.10` and exports complete textual findings to `nikto-report.txt`. |
| `nikto -h http://192.168.56.10 -o nikto-report.html -Format html` | `-o` sets file, `-Format html` specifies HTML layout. | Scans `http://192.168.56.10` and generates a styled HTML vulnerability report in `nikto-report.html`. |

### Common Output Types & Meanings
- **Server Software / Version Disclosure:** Server reveals software details in the HTTP header (e.g., `Apache/2.4.41 (Ubuntu)`), aiding attacker reconnaissance.
- **Outdated Software Warnings:** Flags server software versions with known public CVE vulnerabilities.
- **Interesting Files / Directories:** Discovers publicly accessible administrative paths, backups, or default pages (e.g., `/admin/`, `/phpinfo.php`, `/test.php`).
- **Missing Security Headers:** Flags absence of defensive HTTP headers like `X-Frame-Options` (clickjacking risk) or `X-Content-Type-Options: nosniff`.

> [!CAUTION]
> Nikto actively sends thousands of aggressive HTTP GET/POST requests that trigger firewall alerts and appear prominently in web server error logs. Only scan designated lab VMs (e.g., DVWA, Metasploitable).

---

## 11. Master Command-Line Flag Reference Table

This consolidated reference compares command-line flags and options across all tools. Note that the same flag character can have completely different meanings depending on the tool.

| Flag or Option | Tool | Meaning / Behavior |
| :--- | :--- | :--- |
| `-h` | **WHOIS** | Specifies the remote WHOIS server hostname to query directly (e.g., `-h whois.verisign-grs.com`). |
| `-h` | **theHarvester** | Displays command-line help manual and available search backends. |
| `-h` | **Netdiscover** | Displays command-line help manual and syntax instructions. |
| `-h` | **Nmap** | Displays the command-line help summary for all scanning flags. |
| `-h` | **Nikto** | Specifies the target web server host IP, hostname, or URL to scan. |
| `-d` | **theHarvester** | Specifies the target domain name for OSINT intelligence gathering (e.g., `-d example.com`). |
| `-d` | **Sublist3r** | Specifies the target domain name for subdomain enumeration (e.g., `-d microsoft.com`). |
| `-b` | **theHarvester** | Specifies the search engine or intelligence data source/backend (e.g., `-b crtsh` or `-b google`). |
| `-l` | **theHarvester** | Limits the maximum number of search results returned from data sources. |
| `-q` | **theHarvester** | Enables quiet mode, suppressing informational banner output in terminal. |
| `-o` | **Sublist3r** | Specifies the output text filename to store discovered subdomains (e.g., `-o results.txt`). |
| `-o` | **Nikto** | Specifies the output filename to save scan findings (e.g., `-o nikto-report.txt`). |
| `-p` | **Nmap** | Specifies custom port numbers or port ranges to scan (e.g., `-p 22,80,443,587`). |
| `-p` | **Netdiscover** | Enables **passive listening mode**; sniffs ARP traffic without transmitting packets. |
| `-p` | **Nikto** | Specifies the web server port number to target (e.g., `-p 443`). |
| `-p-` | **Nmap** | Instructs Nmap to scan **all 65,535 TCP ports** (equivalent to `-p 1-65535`). |
| `-sn` | **Nmap** | Performs host discovery only (ping scan) without scanning TCP/UDP ports. |
| `-sS` | **Nmap** | Executes a stealthy TCP SYN (half-open) port scan using raw socket creation (requires `sudo`). |
| `-sV` | **Nmap** | Enables application service and version detection by probing open ports with banner queries. |
| `-O` | **Nmap** | Enables TCP/IP stack fingerprinting for operating system detection (requires `sudo`). |
| `-A` | **Nmap** | Enables aggressive scan mode: combines OS detection, versioning, script scanning, and traceroute. |
| `-i` | **Netdiscover** | Specifies the network interface card to bind and transmit ARP packets on (e.g., `-i eth0`). |
| `-r` | **Netdiscover** | Specifies the target CIDR network range to scan (e.g., `-r 192.168.56.0/24`). |
| `-S` | **Netdiscover** | Enables sleep/delay between packet bursts to prevent network saturation. |
| `-ssl` | **Nikto** | Forces the scanner to initiate an encrypted HTTPS SSL/TLS connection. |
| `-Format` | **Nikto** | Specifies the report output format (e.g., `-Format html`, `-Format txt`, `-Format csv`). |
| `-y` | **APT** | Automatically confirms package installation prompts with "yes" (`sudo apt install <pkg> -y`). |
| `-type=` | **nslookup** | Filters DNS query for a specific resource record type (e.g., `-type=mx`, `-type=ns`, `-type=a`). |
| `-c` | **ping** | Limits the number of ICMP echo request packets sent (e.g., `ping -c 4 1.1.1.1`). |
| `-Version` | **Nikto** | Displays the installed software version and plugin database release. |
| `--version` | **WHOIS / Nmap / etc.** | Displays installed utility version and compilation details. |
| `--help` | **WHOIS / Sublist3r** | Displays syntax documentation and available option flags. |

---

## 12. One-Minute Practical Exam Revision Table

Review this high-yield summary table before entering your laboratory practical examination:

| Tool | Primary Purpose | Most Important Example Command | Scanning Nature |
| :--- | :--- | :--- | :--- |
| **Kali Linux** | Security auditing OS & network verification | `ip a` | Local system diagnostic |
| **WHOIS** | Domain registrar & IP ownership lookup | `whois example.com` | Passive OSINT (TCP 43) |
| **nslookup** | DNS record query & domain resolution | `nslookup -type=mx example.com` | DNS query (UDP/TCP 53) |
| **Nmap** | Host discovery, port scanning & versioning | `nmap -sV 192.168.56.10` | **Active** network scan |
| **theHarvester** | Public email & subdomain OSINT collection | `theHarvester -d microsoft.com -b crtsh` | Passive OSINT query |
| **Netdiscover** | Layer 2 ARP local live host discovery | `sudo netdiscover -r 192.168.56.0/24` | **Active / Passive** ARP (Layer 2) |
| **Sublist3r** | Search engine subdomain enumeration | `sublist3r -d microsoft.com` | Passive search scraping |
| **Nikto** | Web server vulnerability & misconfiguration audit | `nikto -h http://192.168.56.10` | **Active** web probe |

> [!NOTE]
> **Practical Examination Quick Check:**  
> When the examiner asks you to demonstrate a tool:
> 1. Always verify your own IP and interface first: `ip a`.
> 2. Confirm the assigned target IP with your instructor before running active tools (`nmap`, `netdiscover`, `nikto`).
> 3. For OSINT tools (`whois`, `nslookup`, `theHarvester`, `Sublist3r`), check that the VM has outbound internet connectivity (`ping -c 4 1.1.1.1`).
