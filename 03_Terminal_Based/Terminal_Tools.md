# Terminal-Based Ethical Hacking Laboratory Notes
**Course:** ICS1561 – Ethical Hacking Laboratory  
**Category:** 03 – Terminal-Based Tools (Kali Linux)  
**Target Environment:** Kali Linux Virtual Machine / Oracle VirtualBox / Isolated Lab Subnet  
**Topics Covered:** Kali Linux VM, WHOIS, nslookup, Nmap, theHarvester, Netdiscover, Sublist3r, Nikto  
**Reference Manuals:** Kali Installation Guide, Nmap Manual, Reconnaissance Tools, Week 4 (Nikto & DNS), Week 2 Practical

---

## Table of Contents
1. [General Concepts & Command Execution Rules](#1-general-concepts--command-execution-rules)
2. [Topic 1: Kali Linux Virtual Machine](#2-topic-1-kali-linux-virtual-machine)
3. [Topic 2: WHOIS (Domain & IP Registration Lookup)](#3-topic-2-whois-domain--ip-registration-lookup)
4. [Topic 3: nslookup (DNS Query & Record Resolution)](#4-topic-3-nslookup-dns-query--record-resolution)
5. [Topic 4: Nmap (Network Discovery & Port Scanning)](#5-topic-4-nmap-network-discovery--port-scanning)
6. [Topic 5: theHarvester (OSINT Email & Subdomain Enumeration)](#6-topic-5-theharvester-osint-email--subdomain-enumeration)
7. [Topic 6: Netdiscover (Local ARP Network Reconnaissance)](#7-topic-6-netdiscover-local-arp-network-reconnaissance)
8. [Topic 7: Sublist3r (Search Engine Subdomain Enumeration)](#8-topic-7-sublist3r-search-engine-subdomain-enumeration)
9. [Topic 8: Nikto (Web Server Vulnerability Scanner)](#9-topic-8-nikto-web-server-vulnerability-scanner)
10. [Common Package Installation & Troubleshooting Guide](#10-common-package-installation--troubleshooting-guide)
11. [Master Command Reference & Comparison Cheat Sheet](#11-master-command-reference--comparison-cheat-sheet)
12. [One-Minute Lab Record Summary (Aim, Procedure, Observation, Result)](#12-one-minute-lab-record-summary-aim-procedure-observation-result)
13. [Comprehensive Viva Examination Preparation](#13-comprehensive-viva-examination-preparation)

---

## 1. General Concepts & Command Execution Rules

### Elevated Privileges (`sudo`)
- The `sudo` (SuperUser DO) prefix executes administrative commands with root privileges.
- When prompted for a password in Kali Linux, type your account password (default credentials on fresh images are typically `kali` / `kali`). Note that the terminal will not display characters or asterisks while typing.
- Commands requiring raw network socket manipulation (SYN packets, ARP broadcasts, OS fingerprinting) mandate `sudo`.

### Target Placeholders
Whenever commands appear with angled brackets, substitute your authorized laboratory parameters:
- `<authorized-target-IP>` $\rightarrow$ Confirmed IPv4 address of your target VM (e.g., `10.0.2.4` or `192.168.56.102`).
- `<authorized-lab-subnet>` $\rightarrow$ Confirmed CIDR network range of your lab (e.g., `10.0.2.0/24` or `192.168.56.0/24`).
- `<authorized-domain>` $\rightarrow$ Instructor-approved target domain (e.g., `owasp.org` or `ssn.edu.in`).
- `<authorized-lab-web-target>` $\rightarrow$ Target URL or IP hosting an authorized web application (e.g., `http://10.0.2.4/dvwa/`).

---

## 2. Topic 1: Kali Linux Virtual Machine

### Aim
To install, configure, and verify Kali Linux inside Oracle VirtualBox to establish a virtualized security testing environment for ethical hacking laboratory exercises.

### Introduction
Kali Linux is a Debian-based Linux distribution maintained by Offensive Security, specifically tailored for cybersecurity professionals, penetration testers, and digital forensics investigators. It comes pre-installed with hundreds of security auditing tools. In this laboratory, Kali Linux acts as the attacker/analyst workstation operating in an isolated virtual network alongside target systems.

### Installation & VirtualBox Configuration Procedure
*(Note: If you already have a functional Kali Linux VM configured, skip directly to Step 12 for environment verification).*

1. **Download Oracle VirtualBox:** Download the VirtualBox platform package from `https://www.virtualbox.org` and install it on the host operating system.
2. **Download Kali Linux ISO:** Download the pre-built installer ISO (*Kali Linux Installer 64-bit*) from `https://www.kali.org`.
3. **Create New Virtual Machine:** Open VirtualBox, click **New**, and specify:
   - **Name:** `Kali Linux`
   - **Type:** `Linux`
   - **Version:** `Debian (64-bit)`
4. **Allocate Memory & CPU:** Allocate at least 2048 MB (2 GB) RAM (4096 MB recommended) and 2 vCPUs.
5. **Create Virtual Hard Disk:** Create a Virtual Hard Disk using the **VDI (VirtualBox Disk Image)** format with dynamically allocated storage of at least **20 GB** (25–30 GB recommended).
6. **Attach Installation Media:** In VM **Settings** $\rightarrow$ **Storage**, attach the downloaded Kali Linux `.iso` file to the virtual optical drive.
7. **Initiate Graphical Install:** Start the virtual machine and choose **Graphical Install** from the boot menu.
8. **Configure Localization:** Select Language (*English*), Country/Location, and Keymap (*American English*).
9. **Configure Network & Accounts:** Set the hostname (e.g., `kali`), domain name (leave blank), full user name, username (e.g., `kali`), and user password.
10. **Partition Disks:** Choose **Guided - use entire disk**, select the virtual disk, select **All files in one partition**, and confirm changes by selecting **Write the changes to disk** $\rightarrow$ **Yes**.
11. **Complete Installation:** Wait for base system installation, select the default desktop environment (XFCE), install the GRUB boot loader to the primary drive (`/dev/sda`), reboot, and log in.
12. **VirtualBox Network Adapter Modes:**
    - **NAT (Network Address Translation):** VM shares the host IP for outbound access; VM receives `10.0.2.15`.
    - **Host-Only Adapter:** Creates an isolated private network between host and VMs (e.g., `192.168.56.0/24`) without internet routing.
    - **Bridged Adapter:** VM connects directly to the host's physical network, receiving an independent IP from the local router.

---

### Verification Commands & Reference Table

```bash
cat /etc/os-release
ip a
ip route
ping -c 4 1.1.1.1
```

| Command | One-Line Explanation |
| :--- | :--- |
| `cat /etc/os-release` | Displays the installed operating system distribution, release version, and build name. |
| `ip a` | Lists all active network interfaces, hardware MAC addresses, and assigned IP configurations. |
| `ip route` | Displays the kernel network routing table and the active default gateway. |
| `ping -c 4 1.1.1.1` | Sends four ICMP echo requests to test outbound network connectivity and packet latency. |

> *Troubleshooting Note:* Kali is an operating system image; it is **never** installed using `sudo apt install kali`. If `ping 1.1.1.1` returns packet loss, verify VirtualBox network adapter settings and test connectivity against your local gateway (`ip route`).

---

### Observation for the Lab Record
> **Observation (Kali Linux VM Verification):**  
> Kali Linux was installed and started successfully in VirtualBox. The operating system and network configuration were checked using terminal commands. The assigned interface, IP address and routing information were examined. Command `cat /etc/os-release` confirmed Kali Linux Debian-based distribution, `ip a` identified interface `[observed interface, e.g., eth0]` with IP `[observed IP, e.g., 10.0.2.15]`, and `ip route` confirmed the default gateway `[observed gateway, e.g., 10.0.2.2]`.

### Result
The Kali Linux virtual machine was successfully configured in VirtualBox and its operating system identity and network routing parameters were verified.

---

## 3. Topic 2: WHOIS (Domain & IP Registration Lookup)

### Aim
To query public domain name and IP address registry databases using the WHOIS command-line client to retrieve registrar, ownership, and nameserver intelligence.

### Introduction
WHOIS is an Open Source Intelligence (OSINT) protocol and command-line utility used to query regional internet registries (RIRs such as ARIN, RIPE, APNIC) and domain registrars. It returns public record cards detailing domain ownership, administrative contacts, registration timestamps, expiration dates, and authoritative nameservers. Because WHOIS queries third-party registries, it constitutes purely passive reconnaissance.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install whois -y
whois --version
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Refreshes the local APT package repository index with the latest available package lists. |
| `sudo apt install whois -y` | Installs the WHOIS command-line client package from the official Kali repository. |
| `whois --version` | Displays the installed WHOIS client version and software copyright details. |

---

### Practical Commands & Reference Table

```bash
whois --help
whois example.com
whois 8.8.8.8
whois -h whois.verisign-grs.com example.com
```

| Command | One-Line Explanation |
| :--- | :--- |
| `whois --help` | Displays the help manual, syntax format, and available operational flags. |
| `whois example.com` | Queries registry databases for public domain registration, registrar, and nameserver details. |
| `whois 8.8.8.8` | Retrieves Autonomous System Number, network CIDR range, and ISP ownership for the IP address. |
| `whois -h whois.verisign-grs.com example.com` | Queries a specific authoritative registry host directly instead of using automated referral redirects. |

> *Data Privacy Note:* Modern WHOIS queries frequently return redacted records (`REDACTED FOR PRIVACY`) due to GDPR and registrar privacy proxies. This does not indicate tool failure; nameserver, registrar, and date fields typically remain visible.

---

### Observation for the Lab Record
> **Observation (WHOIS Registration Lookup):**  
> WHOIS was used to retrieve the available registration information for the specified domain or IP address. The output was examined for registrar details, registration dates, nameservers and registration status where available. Querying `[target domain / IP]` revealed registrar `[observed registrar]`, creation date `[observed creation date]`, expiration date `[observed expiration date]`, and authoritative name servers `[observed name servers]`.

### Result
Public domain and IP registration records were successfully gathered using the WHOIS command-line tool without sending probe traffic to the target host.

### Viva Questions and Answers
1. **What is WHOIS?**  
   *Answer:* WHOIS is a query and response protocol used to lookup databases that store the registered users or assignees of domain names and autonomous IP address blocks.
2. **What information does a WHOIS query typically disclose?**  
   *Answer:* Domain registrar name, creation/update/expiration dates, domain status codes, authoritative nameservers, and administrative or technical contact details (unless redacted).
3. **What is the function of the `-h` flag in WHOIS?**  
   *Answer:* The `-h` flag specifies a custom WHOIS server hostname to query directly, bypassing the default referral chain.
4. **Why is WHOIS considered passive reconnaissance?**  
   *Answer:* Because the client queries registry database servers (ICANN/RIRs/Registrars) rather than transmitting network packets to the target organization's web or mail servers.
5. **Why are registrant names and personal emails frequently hidden in modern WHOIS records?**  
   *Answer:* Privacy regulations such as the EU General Data Protection Regulation (GDPR) and commercial WHOIS privacy/proxy services redact personal identifying information.

### Quick Revision
- **Tool:** `whois` (Domain & IP registration lookup client).
- **Install:** `sudo apt install whois -y`.
- **Domain Query:** `whois <domain>` (returns registrar, dates, nameservers).
- **IP Query:** `whois <IP>` (returns ASN, netblock CIDR, ISP organization).
- **Server Flag:** `-h <server>` (forces lookup against specific registry).
- **Recon Mode:** Strictly passive footprinting.

---

## 4. Topic 3: nslookup (DNS Query & Record Resolution)

### Aim
To query authoritative and recursive DNS servers using `nslookup` to resolve forward domain mappings, discover mail servers, enumerate nameservers, and perform reverse lookups.

### Introduction
`nslookup` (Name Server Lookup) is a legacy command-line tool used for querying the Domain Name System (DNS) to obtain domain name to IP address mappings and specific resource records (A, MX, NS, PTR, TXT, SOA). It supports both interactive query prompts and non-interactive scripted lookups.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install dnsutils -y
nslookup -version
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Synchronizes local package index files with remote software repositories. |
| `sudo apt install dnsutils -y` | Installs the DNS utilities package providing `nslookup`, `dig`, and `host`. |
| `nslookup -version` | Displays the BIND version information associated with the installed utility suite. |

> *Package Name Note:* In newer Debian/Ubuntu distributions, the transitional package `dnsutils` installs `bind9-dnsutils`. If `dnsutils` fails on a custom system, use `sudo apt install bind9-dnsutils -y`.

---

### Practical Commands & Reference Table

```bash
nslookup
nslookup example.com
nslookup -type=mx example.com
nslookup -type=ns example.com
nslookup example.com 8.8.8.8
nslookup 8.8.8.8
```

| Command | One-Line Explanation |
| :--- | :--- |
| `nslookup` | Launches the interactive DNS lookup prompt for continuous querying (`exit` to quit). |
| `nslookup example.com` | Performs a standard forward DNS lookup, resolving the domain name to its IPv4 (A) or IPv6 (AAAA) addresses. |
| `nslookup -type=mx example.com` | Queries Mail Exchange (MX) resource records, identifying servers responsible for domain email. |
| `nslookup -type=ns example.com` | Queries Name Server (NS) records, identifying the authoritative DNS servers for the zone. |
| `nslookup example.com 8.8.8.8` | Queries the target domain explicitly using the specified external DNS server (`8.8.8.8`). |
| `nslookup 8.8.8.8` | Executes a reverse DNS lookup mapping an IP address back to its canonical hostname via PTR records. |

#### DNS Record Types Explained
- **A Record:** Maps a hostname to an IPv4 address.
- **MX Record:** Directs email to designated mail exchangers with assigned priority weights.
- **NS Record:** Delegates a DNS zone to authoritative nameservers.
- **PTR Record:** Maps an IP address to a canonical hostname in reverse DNS zones (`in-addr.arpa`).

---

### Observation for the Lab Record
> **Observation (nslookup DNS Record Analysis):**  
> nslookup was used to resolve the specified domain and query DNS records. The available IP addresses, mail servers and name servers were examined, and the results were recorded. For target domain `[target domain]`, forward resolution returned IP `[observed IP]`. Querying `-type=mx` identified mail servers `[observed MX servers]`, while `-type=ns` identified authoritative nameservers `[observed nameservers]`. A reverse query on `[target IP]` resolved to `[observed reverse hostname]`.

### Result
DNS forward resolution, mail exchanger discovery, nameserver identification, and reverse lookups were successfully executed using `nslookup`.

### Viva Questions and Answers
1. **What is nslookup?**  
   *Answer:* `nslookup` is a network administration command-line tool used for querying DNS servers to resolve hostnames, IP addresses, and specific resource records.
2. **What does the `-type=mx` parameter do?**  
   *Answer:* It instructs `nslookup` to query specifically for Mail Exchange (MX) records, revealing the mail servers responsible for receiving domain email.
3. **What is the difference between forward and reverse DNS lookups?**  
   *Answer:* A forward lookup maps a human-readable domain name to an IP address (A record), whereas a reverse lookup maps an IP address back to a hostname (PTR record).
4. **Why would an analyst query an external DNS server like `8.8.8.8`?**  
   *Answer:* To bypass poisoned local DNS caches, verify propagation across public resolvers, and compare internal split-horizon DNS views with external views.
5. **What is an authoritative vs. non-authoritative DNS answer?**  
   *Answer:* An authoritative answer comes directly from the server that holds the official zone files for that domain; a non-authoritative answer comes from a resolver's temporary cache.

### Quick Revision
- **Tool:** `nslookup` (bundled in `dnsutils` / `bind9-dnsutils`).
- **Default Query:** `nslookup <domain>` (resolves A/AAAA records).
- **Mail Query:** `nslookup -type=mx <domain>`.
- **Name Server Query:** `nslookup -type=ns <domain>`.
- **Reverse Query:** `nslookup <IP>` (queries PTR in `in-addr.arpa`).
- **Server Override:** `nslookup <domain> <server-IP>` (directs query to specific resolver).

---

## 5. Topic 4: Nmap (Network Discovery & Port Scanning)

### Aim
To discover live hosts, identify open network ports, determine running service versions, and perform operating system fingerprinting on an authorized laboratory target using Nmap.

### Introduction
Nmap (Network Mapper) is a security scanner used for network exploration, host discovery, and vulnerability auditing. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems they are running, and what type of packet filters/firewalls are in use.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install nmap -y
nmap --version
nmap -h
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Refreshes the local software repository index. |
| `sudo apt install nmap -y` | Installs the complete Nmap network scanning suite from official Kali repositories. |
| `nmap --version` | Displays the installed Nmap release version and compilation features. |
| `nmap -h` | Displays the comprehensive Nmap command-line options and flag reference guide. |

> *Verification Note:* Nmap comes pre-installed on Kali Linux. Verify availability with `nmap --version` before attempting re-installation.

---

### Command Corrections from the Lab Manual

```text
========================================================================================
COMMAND CORRECTIONS: MANUAL ERRORS VS. VALID SYNTAX
========================================================================================
1. Error in manual:  nmap -p- 22,80,443,587
   Explanation:      The flag '-p-' instructs Nmap to scan ALL 65,535 TCP ports. Appending
                     a comma-separated port list to '-p-' is invalid syntax.
   Correct Selected: nmap -p 22,80,443,587 <target>    (Scans only the 4 specified ports)
   Correct All Ports: nmap -p- <target>                 (Scans all 65,535 TCP ports)

2. Error in manual:  nmap --h
   Explanation:      '--h' is non-standard syntax that errors out or prints usage.
   Correct Version:  nmap --version                    (Prints Nmap version)
   Correct Help:     nmap -h                           (Prints command-line help)
========================================================================================
```

---

### Practical Scanning Workflow & Reference Table

Follow this structured execution order during laboratory assessments:
1. Identify the target IP address or subnet and verify it belongs to the authorized lab environment.
2. Check your local Kali network configuration using `ip a` and `ip route`.
3. Perform host discovery (`-sn`) across the subnet to locate active devices.
4. Execute a basic port scan against identified live hosts.
5. Perform targeted port scanning (`-p`) or full-range scanning (`-p-`).
6. Perform service version detection (`-sV`) on open ports.
7. Perform OS detection (`-O`) with elevated privileges (`sudo`).
8. Combine scanning flags (`-sS -sV -O` or `-A`) for comprehensive host profiling.
9. Analyze open ports, service versions, and OS confidence levels.

```bash
nmap -sn <authorized-lab-subnet>
nmap <authorized-target-IP>
nmap -p 22,80,443,587 <authorized-target-IP>
nmap -p- <authorized-target-IP>
sudo nmap -sS <authorized-target-IP>
sudo nmap -O <authorized-target-IP>
nmap -sV <authorized-target-IP>
sudo nmap -sS -sV -O <authorized-target-IP>
sudo nmap -A <authorized-target-IP>
```

| Command | One-Line Explanation |
| :--- | :--- |
| `nmap -sn <authorized-lab-subnet>` | Performs host discovery (ping sweep) across the subnet without scanning ports. |
| `nmap <authorized-target-IP>` | Scans the top 1,000 most common TCP ports on the specified target host. |
| `nmap -p 22,80,443,587 <authorized-target-IP>` | Scans only the specific comma-separated port numbers on the target host. |
| `nmap -p- <authorized-target-IP>` | Scans all 65,535 TCP ports on the target host. |
| `sudo nmap -sS <authorized-target-IP>` | Executes a stealthy TCP SYN (half-open) scan requiring root privileges. |
| `sudo nmap -O <authorized-target-IP>` | Analyzes low-level TCP/IP stack responses to fingerprint the underlying operating system. |
| `nmap -sV <authorized-target-IP>` | Probes open ports to detect specific application daemon names and version numbers. |
| `sudo nmap -sS -sV -O <authorized-target-IP>` | Combines TCP SYN scanning, service version detection, and OS fingerprinting in one command. |
| `sudo nmap -A <authorized-target-IP>` | Enables aggressive scanning, combining OS detection, version scanning, script scanning (NSE), and traceroute. |

---

### Output Interpretation & Port States

| Port State | Technical Meaning |
| :--- | :--- |
| **Open** | An application or service is actively accepting TCP connections or UDP packets on this port. |
| **Closed** | Port is accessible (receives probe packets and responds with RST), but no application is listening. |
| **Filtered** | A firewall, packet filter, or router rule prevents probe packets from reaching the port; Nmap cannot determine if it is open or closed. |
| **Unfiltered** | Port is accessible, but Nmap cannot determine whether it is open or closed (seen in ACK scans). |

> *Crucial Distinction:* An **open port indicates an accessible service**, not an exploitable vulnerability. A service is only vulnerable if it runs outdated, misconfigured, or flaw-ridden software.

---

### Observation for the Lab Record
> **Observation (Nmap Network Scan):**  
> Nmap was used to discover hosts and examine the network ports of the authorised laboratory target. The scan output was reviewed to identify host status, open ports and available service information. Host discovery (`-sn`) on subnet `[observed subnet]` revealed `[observed live hosts count]` live systems. A targeted scan on host `[observed target IP]` identified open ports `[observed open ports, e.g., 22/tcp, 80/tcp, 445/tcp]` running services `[observed services, e.g., OpenSSH, Apache, Samba]`. Operating system fingerprinting identified the target as `[observed OS guess]` with `[observed confidence %]` accuracy.

### Result
Active host discovery, port scanning, service version enumeration, and operating system fingerprinting were successfully performed on the authorized lab target using Nmap.

### Viva Questions and Answers
1. **What is Nmap and what is its primary use?**  
   *Answer:* Nmap is an open-source network scanner used for network inventory, host discovery, port scanning, service version detection, and operating system fingerprinting.
2. **What is the difference between `nmap -sT` and `nmap -sS`?**  
   *Answer:* `-sT` performs a full TCP 3-way handshake (SYN, SYN-ACK, ACK) and does not require root privileges, but is logged by target applications. `-sS` (SYN scan) sends SYN and responds with RST upon receiving SYN-ACK (half-open), making it faster, stealthier, and requiring `sudo`.
3. **What does the `-sV` option do?**  
   *Answer:* It enables service and version detection by querying open ports with application probes and matching banners against Nmap's signature database.
4. **What does the `-sn` flag accomplish?**  
   *Answer:* It tells Nmap to skip port scanning and perform host discovery only (ping scan), reporting which IP addresses on a subnet are responsive.
5. **Does an open port imply a system is compromised or vulnerable?**  
   *Answer:* No. An open port simply indicates a listening service accepting network connections. It is only vulnerable if that specific service version contains an unpatched security defect or misconfiguration.

### Quick Revision
- **Tool:** `nmap` (Network discovery and security auditing suite).
- **Ping Sweep:** `nmap -sn <subnet>` (host discovery without port scans).
- **Default Scan:** `nmap <IP>` (scans top 1,000 TCP ports).
- **Port Flags:** `-p <ports>` (specific ports), `-p-` (all 65,535 ports).
- **SYN Scan:** `sudo nmap -sS <IP>` (stealthy half-open scan).
- **Version & OS:** `-sV` (service versions), `-O` (OS detection).
- **Aggressive:** `-A` (combines OS, version, script scanning, traceroute).
- **Safety Policy:** Scan **only** confirmed lab targets; never unauthorized public systems.

---

## 6. Topic 5: theHarvester (OSINT Email & Subdomain Enumeration)

### Aim
To gather public open-source intelligence on a target domain using `theHarvester`, collecting publicly indexed email addresses, employee names, subdomains, and host IP addresses.

### Introduction
`theHarvester` is an Open Source Intelligence (OSINT) reconnaissance tool developed in Python. It aggregates data from multiple public search engines, PGP key servers, Shodan, and Certificate Transparency logs (`crt.sh`) to catalog domain subdomains, employee email addresses, virtual hosts, and open ports. It functions as an essential passive footprinting tool during the initial intelligence gathering phase.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install theharvester -y
theHarvester --version
theHarvester -h
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Updates package lists from remote software archives. |
| `sudo apt install theharvester -y` | Installs theHarvester and required Python dependencies from the Kali repository. |
| `theHarvester --version` | Displays the installed release version of theHarvester. |
| `theHarvester -h` | Displays the help menu, accepted arguments, and supported search engine sources. |

---

### Practical Commands & Reference Table

```bash
theHarvester -d <authorized-domain> -b <source>
theHarvester -d example.com -b google
theHarvester -d example.com -b crtsh
theHarvester -d example.com -b crtsh,hackertarget -l 20
theHarvester -d microsoft.com -b all -l 300 -q
```

| Command | One-Line Explanation |
| :--- | :--- |
| `theHarvester -d <authorized-domain> -b <source>` | Queries the specified data source for public records associated with the target domain. |
| `theHarvester -d example.com -b google` | Scrapes Google search engine index results for public emails and subdomains of `example.com`. |
| `theHarvester -d example.com -b crtsh` | Queries the Certificate Transparency log database (`crt.sh`) to uncover SSL/TLS subdomains. |
| `theHarvester -d example.com -b crtsh,hackertarget -l 20` | Queries multiple sources simultaneously while limiting the search volume to 20 results. |
| `theHarvester -d microsoft.com -b all -l 300 -q` | Queries all available search engines with a limit of 300 results in quiet mode (`-q`). |

#### Flag Reference Guide
- `-d <domain>`: Specifies the target domain to harvest.
- `-b <source>`: Specifies the search engine or backend provider (e.g., `google`, `bing`, `crtsh`, `hackertarget`, `all`).
- `-l <limit>`: Sets the query retrieval limit.
- `-q`: Enables quiet mode, suppressing banner art.

> *Data Source Availability Warning:* Third-party search engine APIs frequently implement CAPTCHA blocks or deprecate scraper endpoints. If a source like `google` returns zero results or rate-limit warnings, test Certificate Transparency sources (`-b crtsh`), which query public TLS certificate records and remain highly reliable. A result of zero items does **not** prove a domain has no subdomains.

---

### Observation for the Lab Record
> **Observation (theHarvester OSINT Gathering):**  
> theHarvester was used to collect publicly available information associated with the selected domain. The results were examined for email addresses, subdomains, hostnames and IP addresses where available. Scanning domain `[target domain]` using data sources `[observed sources, e.g., crtsh, google]` identified `[observed subdomains count]` subdomains (e.g., `[observed sample subdomains]`) and `[observed emails count]` public email addresses. All queries were processed against third-party public repositories.

### Result
Passive open-source footprinting was successfully executed using `theHarvester`, retrieving indexed subdomains and public email addresses without sending packets directly to the target infrastructure.

### Viva Questions and Answers
1. **What is the primary function of theHarvester?**  
   *Answer:* To passively harvest public email addresses, subdomains, employee names, and virtual hosts associated with a target domain from open search engines and public databases.
2. **What does the `-b` option specify in theHarvester?**  
   *Answer:* The `-b` option specifies the data source or backend engine to search, such as `google`, `bing`, `duckduckgo`, `crtsh`, or `all`.
3. **What is Certificate Transparency (`crt.sh`) and why is it useful in OSINT?**  
   *Answer:* Certificate Transparency is an open framework of public audit logs recording all issued SSL/TLS certificates; querying it exposes historical and newly created subdomains.
4. **Why might theHarvester return zero results for a known organization?**  
   *Answer:* Commercial search engines often block automated scraping via CAPTCHAs, rate limiting, or updated HTML structures, requiring alternate sources or API keys.
5. **Is using theHarvester considered active or passive reconnaissance?**  
   *Answer:* It is passive reconnaissance because queries are submitted to third-party databases and search engines, not to the target organization's servers.

### Quick Revision
- **Tool:** `theHarvester` (OSINT email and subdomain harvester).
- **Core Command:** `theHarvester -d <domain> -b <source>`.
- **Top Sources:** `crtsh` (Certificate Transparency), `hackertarget`, `bing`.
- **Limit Flag:** `-l <num>` (constrains result counts).
- **Quiet Flag:** `-q` (clean terminal output).
- **Result Verification:** Discovered hosts must be verified with DNS lookups.

---

## 7. Topic 6: Netdiscover (Local ARP Network Reconnaissance)

### Aim
To scan an authorized local area network segment using Netdiscover, discovering active hosts and mapping their IP addresses, hardware MAC addresses, and device vendor identities via ARP requests.

### Introduction
Netdiscover is an active/passive network address discovery tool designed for wireless and local switched networks without a DHCP server. It relies on the Address Resolution Protocol (ARP), sending ARP requests and sniffing for ARP replies to identify active hosts on Layer 2 segments. Because ARP is a non-routable protocol, Netdiscover is strictly used on local, directly connected subnets.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install netdiscover -y
netdiscover -h
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Refreshes the local repository index before software installation. |
| `sudo apt install netdiscover -y` | Installs the Netdiscover ARP scanner package from the Kali software repository. |
| `netdiscover -h` | Displays the help menu, argument syntax, and scanning options. |

---

### Practical Commands & Reference Table

```bash
sudo netdiscover
sudo netdiscover -r <authorized-lab-subnet>
sudo netdiscover -i eth0
sudo netdiscover -i wlan0 -r <authorized-lab-subnet>
sudo netdiscover -p
```

| Command | One-Line Explanation |
| :--- | :--- |
| `sudo netdiscover` | Scans the local network segment using the default network interface. |
| `sudo netdiscover -r <authorized-lab-subnet>` | Scans the specified CIDR network range (e.g., `10.0.2.0/24` or `192.168.56.0/24`). |
| `sudo netdiscover -i eth0` | Binds the ARP scanning engine explicitly to interface `eth0`. |
| `sudo netdiscover -i wlan0 -r <authorized-lab-subnet>` | Scans the specified IP range over a wireless interface (`wlan0`). |
| `sudo netdiscover -p` | Enables passive mode, silently sniffing ARP traffic without transmitting any ARP request packets. |

> *Note on `-S` Option from Manual:* Older lab manuals reference `netdiscover -S`, which was historically used to sleep between packets to throttle scanning. In modern versions, inspect `netdiscover -h` to verify supported timing flags (`-s <time>`).

---

### VirtualBox Network Configuration Warning

```text
========================================================================================
VIRTUALBOX NETWORK MODES & SCANNING IMPLICATIONS
========================================================================================
1. NAT (Default):
   The VM resides behind VirtualBox's internal NAT router, receiving an address like
   10.0.2.15. Scanning 10.0.2.0/24 discovers VirtualBox virtual nodes (10.0.2.2, 10.0.2.3).

2. Host-Only Adapter:
   Creates an isolated internal virtual network on the host (e.g., 192.168.56.0/24).
   Ideal for scanning local lab virtual machines (Kali, Metasploitable, Windows XP).

3. Bridged Adapter:
   The VM attaches directly to your physical host network (e.g., home Wi-Fi or college
   LAN), receiving an IP from your physical router (e.g., 192.168.1.0/24).
   WARNING: Do not run active ARP scans across unauthorized public or campus Wi-Fi networks!
========================================================================================
```

#### Mandatory Pre-Scan Verification Steps
Before executing Netdiscover:
1. Run `ip a` to check your active network interface name and IP address.
2. Run `ip route` to confirm your subnet range and gateway.
3. Confirm that the subnet belongs exclusively to your instructor-authorized lab environment.

---

### Observation for the Lab Record
> **Observation (Netdiscover ARP Discovery):**  
> Netdiscover was used to identify live hosts in the authorised local laboratory network. The output was examined to record the available IP addresses, MAC addresses and related host information. An active ARP scan on subnet `[observed lab subnet, e.g., 10.0.2.0/24]` via interface `[observed interface, e.g., eth0]` detected `[observed host count]` active devices. Identified IP addresses included `[observed IPs]` with corresponding MAC addresses `[observed MACs]` identified as `[observed vendor, e.g., PCS Systemtechnik / VirtualBox virtual NIC]`.

### Result
Local network host discovery and Layer 2 MAC address mapping were successfully executed on the authorized virtual subnet using Netdiscover.

### Viva Questions and Answers
1. **What is Netdiscover and how does it find live hosts?**  
   *Answer:* Netdiscover is an ARP reconnaissance tool that discovers active devices on a local subnet by transmitting ARP requests ("Who has IP X? Tell IP Y") and capturing incoming ARP replies.
2. **Why does Netdiscover require root (`sudo`) privileges?**  
   *Answer:* It creates raw Layer 2 network sockets to craft, transmit, and inject low-level ARP packets directly onto the physical/virtual network interface.
3. **What is the difference between active and passive mode in Netdiscover?**  
   *Answer:* Active mode (`-r`) actively transmits ARP request packets across the subnet to solicit responses; passive mode (`-p`) silently listens for broadcast ARP packets without sending any probe traffic.
4. **Why can Netdiscover only scan local network segments?**  
   *Answer:* ARP operates at Layer 2 (Data Link layer) and broadcast frames do not pass through Layer 3 routers; Netdiscover cannot scan across routers or the internet.
5. **How does Netdiscover determine the hardware vendor of a discovered device?**  
   *Answer:* It parses the first 3 bytes (24 bits) of the discovered MAC address, known as the Organizationally Unique Identifier (OUI), and matches it against IEEE registry tables.

### Quick Revision
- **Tool:** `netdiscover` (Layer 2 ARP reconnaissance tool).
- **Elevation:** Always requires `sudo`.
- **Active Scan:** `sudo netdiscover -r <subnet>` (injects ARP requests).
- **Passive Sniffing:** `sudo netdiscover -p` (silent, zero packet transmission).
- **Interface Flag:** `-i <interface>` (e.g., `-i eth0`).
- **Data Captured:** IP Address, MAC Address, MAC Vendor, Packet Count.
- **Scope Limit:** Operates only within local broadcast domains.

---

## 8. Topic 7: Sublist3r (Search Engine Subdomain Enumeration)

### Aim
To enumerate public subdomains for an authorized target domain using `Sublist3r` across major public search engines and online repositories.

### Introduction
Sublist3r is a Python-based Open Source Intelligence (OSINT) utility designed to enumerate subdomains of websites using OSINT. It scours multiple popular public search engines (Google, Bing, Yahoo, Baidu, Ask) as well as Netcraft, Virustotal, ThreatCrowd, DNSdumpster, and ReverseDNS to discover publicly accessible subdomains and expand the attack surface mapping of a target organization.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install sublist3r -y
sublist3r --help
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Refreshes the local repository catalog. |
| `sudo apt install sublist3r -y` | Installs Sublist3r and necessary Python dependencies from the Kali package repository. |
| `sublist3r --help` | Displays the help menu, supported options, and operational switches. |

> *Maintenance & Legacy Note:* Sublist3r is a legacy Python tool. Because search engine layouts evolve, certain integrated search scrapers may return errors or zero entries. If specific search engines fail, Sublist3r continues querying the remaining available providers.

---

### Practical Commands & Reference Table

```bash
sublist3r -d <authorized-domain>
sublist3r -d microsoft.com
sublist3r -d microsoft.com -o results.txt
cat results.txt
```

| Command | One-Line Explanation |
| :--- | :--- |
| `sublist3r -d <authorized-domain>` | Executes multi-engine OSINT subdomain discovery for the specified authorized domain. |
| `sublist3r -d microsoft.com` | Queries public search engines for subdomains belonging to the example domain `microsoft.com`. |
| `sublist3r -d microsoft.com -o results.txt` | Discovers subdomains and exports the sorted, unique list to a local text file named `results.txt`. |
| `cat results.txt` | Displays the contents of the exported subdomain text file directly in the terminal. |

---

### Observation for the Lab Record
> **Observation (Sublist3r Subdomain Enumeration):**  
> Sublist3r was used to enumerate publicly available subdomains associated with the specified domain. The discovered subdomains were displayed and saved to a text file for review. Scanning target domain `[target domain, e.g., ssn.edu.in / microsoft.com]` across public search engines returned `[observed subdomains count]` unique subdomains (such as `[sample subdomains]`). Output was written to `results.txt` and verified using `cat`.

### Result
Passive subdomain enumeration was successfully performed using Sublist3r, compiling a validated list of public subdomains without generating traffic against the target's web server.

### Viva Questions and Answers
1. **What is Sublist3r?**  
   *Answer:* Sublist3r is a Python OSINT tool designed to enumerate subdomains of a target domain by scraping multiple public search engines and specialized internet data platforms.
2. **What does the `-o` parameter do in Sublist3r?**  
   *Answer:* The `-o` parameter specifies a text filename to save the discovered, de-duplicated list of subdomains.
3. **Why is subdomain enumeration important in ethical hacking?**  
   *Answer:* Subdomain enumeration expands the organizational attack surface, discovering forgotten development environments, unlinked portals, and staging servers that may have weaker security controls.
4. **How does Sublist3r discover subdomains without performing brute-force DNS guessing by default?**  
   *Answer:* By querying public search engine caches and indexing databases (Google, Bing, Netcraft, VirusTotal) for existing references to hostnames ending with the target domain.
5. **What should an analyst do if an older tool like Sublist3r encounters scraper errors?**  
   *Answer:* Rely on Certificate Transparency databases (`crt.sh`), newer tools (such as `amass` or `subfinder`), or verify outputs by cross-referencing with `nslookup`.

### Quick Revision
- **Tool:** `sublist3r` (Search engine subdomain scraper).
- **Core Command:** `sublist3r -d <domain>`.
- **Output File:** `sublist3r -d <domain> -o <filename.txt>`.
- **View Output:** `cat <filename.txt>`.
- **Data Engine:** Scrapes Google, Bing, Yahoo, Baidu, Netcraft, and VirusTotal.
- **Recon Mode:** Passive reconnaissance.

---

## 9. Topic 8: Nikto (Web Server Vulnerability Scanner)

### Aim
To scan an authorized laboratory web server using Nikto to audit server software versions, identify dangerous files, detect missing HTTP security headers, and evaluate web server configuration weaknesses.

### Introduction
Nikto is an open-source command-line web server scanner that performs comprehensive tests against web servers for multiple vulnerabilities. It inspects targets for over 6,700 potentially dangerous files and CGIs, checks for outdated server software versions, audits configuration settings, and tests for server response weaknesses. Nikto operates actively by transmitting HTTP/HTTPS requests to the target web server.

---

### Installation Commands & Reference Table

```bash
sudo apt update
sudo apt install nikto -y
nikto -Version
```

| Installation Command | One-Line Explanation |
| :--- | :--- |
| `sudo apt update` | Updates package definitions from the distribution repository. |
| `sudo apt install nikto -y` | Installs Nikto and its Perl library dependencies from the Kali repository. |
| `nikto -Version` | Displays the installed Nikto scanner release and plugin database version. |

> *Verification Note:* Nikto is pre-installed in standard Kali Linux distributions. Verify availability by typing `nikto -Version` before attempting re-installation.

---

### Practical Scanning Workflow & Reference Table

Execute Nikto scans strictly in this structured sequence:
1. Confirm the IP address or URL of the assigned, authorized training web server (e.g., local DVWA or Metasploitable VM).
2. Verify network connectivity using `ping -c 2 <target-IP>`.
3. Launch a basic scan against the target HTTP port (port 80).
4. Run targeted scans specifying port numbers or SSL/TLS encryption where applicable.
5. Allow the scan to complete naturally and avoid aborting early.
6. Export the findings to a structured report file (TXT or HTML).
7. Review findings line by line (each prefixed with a plus sign `+`).

```bash
nikto -h <authorized-lab-web-target>
nikto -h <authorized-lab-web-target> -p 443 -ssl
nikto -h <authorized-lab-web-target> -o report.txt
nikto -h <authorized-lab-web-target> -o report.html -Format html
```

| Command | One-Line Explanation |
| :--- | :--- |
| `nikto -h <authorized-lab-web-target>` | Executes a baseline web vulnerability scan against the target host on default port 80. |
| `nikto -h <authorized-lab-web-target> -p 443 -ssl` | Scans the target on port 443, forcing SSL/HTTPS encrypted communication. |
| `nikto -h <authorized-lab-web-target> -o report.txt` | Runs the vulnerability scan and exports the output to a plain text file (`report.txt`). |
| `nikto -h <authorized-lab-web-target> -o report.html -Format html` | Runs the vulnerability scan and exports an HTML-formatted report for browser viewing. |

---

### Interpreting Nikto Output

Nikto outputs each security observation on an individual line beginning with a plus sign (`+`):
1. **Server Header Disclosure:** Discloses exact web server software and operating system versions (e.g., `Server: Apache/2.4.29 (Ubuntu)`), allowing attackers to research known CVEs.
2. **Missing Anti-Clickjacking Headers:** Flags absent `X-Frame-Options` headers, which could allow malicious framing of web pages.
3. **Missing MIME-Sniffing Protections:** Flags absent `X-Content-Type-Options: nosniff` headers.
4. **Insecure Cookie Attributes:** Identifies session cookies created without `HttpOnly` or `Secure` flags.
5. **Accessible Default Files:** Discloses readable `robots.txt`, sample scripts, test directories, or default administrative login portals.

> *Crucial Security Finding Note:* Nikto findings are **leads and indicators requiring verification**, not automated proof of successful exploitation. High-traffic scans can trigger web application firewalls (WAF) or generate false positives. Always validate findings manually before reporting.

#### Strict Safety Mandate
Nikto sends thousands of active HTTP probe requests that generate extensive web server access logs. **Scan only authorized laboratory targets (e.g., DVWA, Metasploitable). Never scan external or public websites.**

---

### Observation for the Lab Record
> **Observation (Nikto Web Vulnerability Scan):**  
> Nikto was used to scan the authorised laboratory web server for potential issues and misconfigurations. The output was examined for server information, outdated software warnings, accessible files and security-header findings. Scanning target `[observed target IP / URL]` on port `[observed port, e.g., 80]` reported `[observed findings count]` total items. Notable findings included web server banner disclosure (`[observed server header]`), missing `X-Frame-Options` and `X-Content-Type-Options` headers, and readable entries in `robots.txt`. Findings were exported to `report.txt`.

### Result
Automated web server configuration auditing and vulnerability scanning were successfully executed against the authorized lab server using Nikto, generating structured findings and exported reports.

### Viva Questions and Answers
1. **What is Nikto and what does it scan?**  
   *Answer:* Nikto is an open-source command-line web server assessment tool that scans web servers for dangerous files, outdated server software, security misconfigurations, and missing HTTP headers.
2. **What does the `-ssl` switch signify in Nikto?**  
   *Answer:* The `-ssl` switch forces Nikto to initiate an encrypted HTTPS connection to the target port, which is required when scanning SSL/TLS-enabled web services.
3. **Why are missing HTTP headers (like `X-Frame-Options`) flagged by Nikto?**  
   *Answer:* Missing `X-Frame-Options` headers leave web applications vulnerable to Clickjacking attacks, where an attacker embeds the vulnerable page in a transparent `<iframe>` to trick users into clicking buttons.
4. **How does Nikto identify an outdated web server version?**  
   *Answer:* It examines the HTTP `Server` response header returned by the host and cross-references the reported version string against its internal database of known vulnerable versions.
5. **Why should Nikto scans only be run on authorized systems?**  
   *Answer:* Because Nikto actively transmits thousands of intrusive HTTP requests seeking dangerous files and CGIs, which causes high server load, triggers intrusion detection alerts, and may be considered an illegal attack if unapproved.

### Quick Revision
- **Tool:** `nikto` (Command-line web server scanner).
- **Core Syntax:** `nikto -h <target>`.
- **Port & SSL:** `-p <port> -ssl` (e.g., `nikto -h <target> -p 443 -ssl`).
- **File Output:** `-o <file> -Format <txt|html|csv>`.
- **Finding Format:** Each finding begins with `+`.
- **Common Findings:** Server version disclosure, missing security headers (`X-Frame-Options`), exposed `robots.txt`.
- **Operation Mode:** Active HTTP scanning requiring explicit authorization.

---

## 10. Common Package Installation & Troubleshooting Guide

### Why `sudo apt update` Precedes Installation
Running `sudo apt update` refreshes the local package index cache from official distribution repositories. Without running `apt update`, the system attempts to download packages using outdated package URLs or version numbers, frequently causing `404 Not Found` or `Unable to locate package` errors.

### Handling Kali Password Prompts
When executing `sudo`, Linux prompts for your account password. Type the password calmly; the cursor will not advance or show masking characters. Press **Enter** upon completion.

### Verifying Successful Installation
Never assume a command succeeded without inspecting output. Verify installation using the POSIX standard shell command:
```bash
command -v <tool-name>
```
*Example:* `command -v nmap` returns `/usr/bin/nmap` if installed, or no output if absent.

### Inspecting Syntax with `--help` or `-h`
Third-party utilities and package versions undergo syntax changes across operating system releases. Always inspect supported options via:
```bash
<tool-name> --help   # or <tool-name> -h
```

### Safe Diagnostic Workflow for Errors
1. **"Package not found":** Run `sudo apt update`, verify correct spelling, or search repositories via `apt search <keyword>`.
2. **"Permission denied":** Prepend `sudo` to the command to elevate privileges.
3. **"Network unreachable":** Run `ip a` and `ip route` to check interface states and gateway reachability.
4. **"Connection refused":** Verify that the target service daemon is started and accepting traffic.

---

## 11. Master Command Reference & Comparison Cheat Sheet

### Comprehensive Tool Reference Matrix

| Tool | Category | Installation Command | Main Practical Command | Main Output Produced |
| :--- | :--- | :--- | :--- | :--- |
| **Kali Linux** | Operating System | Setup via VirtualBox & Kali ISO | `ip a && ip route` | Active network interfaces, IP addresses, and kernel routes |
| **WHOIS** | Passive OSINT | `sudo apt install whois -y` | `whois <authorized-domain>` | Registrar name, registration dates, nameservers, status |
| **nslookup** | DNS Resolution | `sudo apt install dnsutils -y` | `nslookup -type=mx <domain>` | Domain IP addresses, mail servers (MX), nameservers (NS) |
| **Nmap** | Active Scanning | `sudo apt install nmap -y` | `sudo nmap -sS -sV -O <target>` | Live hosts, open TCP/UDP ports, service versions, OS guess |
| **theHarvester** | Passive OSINT | `sudo apt install theharvester -y` | `theHarvester -d <domain> -b crtsh` | Public email addresses, employee names, subdomains |
| **Netdiscover** | Local ARP Scan | `sudo apt install netdiscover -y` | `sudo netdiscover -r <subnet>` | Local host IP addresses, MAC addresses, MAC vendors |
| **Sublist3r** | Search Engine OSINT | `sudo apt install sublist3r -y` | `sublist3r -d <domain> -o out.txt` | Comprehensive list of indexed subdomains |
| **Nikto** | Web Vulnerability | `sudo apt install nikto -y` | `nikto -h <target> -o report.txt` | Web server vulnerabilities, missing headers, accessible files |

---

### Compact Command Reference Summary

```text
========================================================================================
ETHICAL HACKING TERMINAL COMMANDS CHEAT SHEET
========================================================================================
1. KALI LINUX VM:
   ip a                                -> Check IP address and interface configuration
   ip route                            -> Check default gateway and routing subnet

2. WHOIS:
   whois <domain>                      -> Look up domain registration and nameservers
   whois <IP>                          -> Look up IP ownership and Autonomous System Number

3. NSLOOKUP:
   nslookup <domain>                   -> Resolve domain to IPv4/IPv6 addresses
   nslookup -type=mx <domain>          -> Discover mail exchanger servers
   nslookup <IP>                       -> Perform reverse DNS lookup (PTR)

4. NMAP:
   nmap -sn <subnet>                   -> Host discovery ping sweep (no port scan)
   nmap -p <ports> <target>            -> Scan specific ports (e.g. 22,80,443)
   sudo nmap -sS -sV -O <target>       -> Stealth SYN scan with version and OS detection

5. THEHARVESTER:
   theHarvester -d <domain> -b crtsh   -> Harvest subdomains via Certificate Transparency
   theHarvester -d <domain> -b google  -> Scrape search results for public email addresses

6. NETDISCOVER:
   sudo netdiscover -r <subnet>        -> Active ARP scan across local subnet
   sudo netdiscover -p                 -> Passive ARP sniffing (zero packet injection)

7. SUBLIST3R:
   sublist3r -d <domain> -o out.txt    -> Enumerate subdomains and export to text file

8. NIKTO:
   nikto -h <web-target>               -> Scan web server for misconfigurations & vulnerabilities
   nikto -h <web-target> -p 443 -ssl   -> Force SSL/HTTPS scanning on port 443
========================================================================================
```

---

## 12. One-Minute Lab Record Summary (Aim, Procedure, Observation, Result)

Use these short summaries for rapid entry into handwritten practical records during laboratory examinations:

### 1. Kali Linux Virtual Machine
- **Aim:** To configure and verify Kali Linux in VirtualBox for laboratory exercises.
- **Procedure:** Start Kali VM in VirtualBox, log in, execute `ip a` to note interface IP, check routing table with `ip route`, and test connectivity with `ping -c 4 1.1.1.1`.
- **Observation:** Kali Linux started successfully; command `ip a` identified interface `[observed interface]` with IP `[observed IP]`, and `ip route` confirmed the default gateway `[observed gateway]`.
- **Result:** Kali Linux virtual machine and local network routing were successfully configured and verified.

### 2. WHOIS
- **Aim:** To retrieve domain and IP registration information using the WHOIS command-line tool.
- **Procedure:** Open terminal, execute `whois <authorized-domain>`, and inspect the returned registrar, creation date, and nameservers.
- **Observation:** WHOIS query for `[target domain]` returned registrar `[observed registrar]`, creation date `[observed date]`, and authoritative nameservers `[observed nameservers]`.
- **Result:** Public domain registration details were successfully retrieved using WHOIS.

### 3. nslookup
- **Aim:** To resolve domain names and query specific DNS resource records using nslookup.
- **Procedure:** Run `nslookup <target-domain>` for A records, `nslookup -type=mx <target-domain>` for mail servers, and `nslookup <target-IP>` for reverse PTR lookup.
- **Observation:** nslookup resolved `[target domain]` to IP `[observed IP]`, identified mail servers `[observed MX servers]`, and performed reverse resolution on `[target IP]`.
- **Result:** Forward, mail exchange, and reverse DNS records were successfully resolved using nslookup.

### 4. Nmap
- **Aim:** To perform host discovery, port scanning, service enumeration, and OS detection on an authorized target using Nmap.
- **Procedure:** Run `nmap -sn <subnet>` for host discovery, followed by `sudo nmap -sS -sV -O <target-IP>` to scan ports, detect service versions, and identify the operating system.
- **Observation:** Nmap discovered host `[observed IP]`; port scanning revealed open ports `[observed open ports]` running services `[observed services]` under operating system `[observed OS]`.
- **Result:** Network port scanning, service version enumeration, and OS fingerprinting were successfully completed using Nmap.

### 5. theHarvester
- **Aim:** To harvest public email addresses and subdomains for a target domain using theHarvester.
- **Procedure:** Run `theHarvester -d <authorized-domain> -b crtsh` in terminal, wait for analysis, and review discovered hostnames and emails.
- **Observation:** theHarvester queried public Certificate Transparency logs for `[target domain]`, discovering `[observed count]` subdomains including `[sample subdomains]`.
- **Result:** Public subdomains and OSINT intelligence were successfully gathered using theHarvester.

### 6. Netdiscover
- **Aim:** To discover active hosts on the local network segment using Netdiscover ARP requests.
- **Procedure:** Run `sudo netdiscover -r <authorized-lab-subnet>` in terminal, monitor ARP replies, and record discovered IPs and MAC addresses.
- **Observation:** Netdiscover detected `[observed count]` active hosts on subnet `[observed subnet]`, mapping IP `[observed IP]` to MAC `[observed MAC]` (`[observed vendor]`).
- **Result:** Active local hosts and Layer 2 MAC addresses were successfully discovered using Netdiscover.

### 7. Sublist3r
- **Aim:** To enumerate website subdomains using Sublist3r across public search engines.
- **Procedure:** Run `sublist3r -d <authorized-domain> -o results.txt`, wait for search engine queries to complete, and review `results.txt`.
- **Observation:** Sublist3r searched multiple search engines for `[target domain]`, compiling `[observed count]` unique subdomains into `results.txt`.
- **Result:** Subdomain enumeration across search engines was successfully performed using Sublist3r.

### 8. Nikto
- **Aim:** To scan an authorized web server for vulnerabilities and misconfigurations using Nikto.
- **Procedure:** Run `nikto -h <authorized-lab-web-target> -o report.txt` in terminal, wait for scan completion, and review findings in `report.txt`.
- **Observation:** Nikto scanned web server `[target IP/URL]`, identifying `[observed count]` issues including server header disclosure (`[observed server]`) and missing security headers.
- **Result:** Web server vulnerability scanning and security header auditing were successfully performed using Nikto.

---

## 13. Comprehensive Viva Examination Preparation

### One-Minute Viva Rapid Revision Table

| Tool | Core Viva Definition | Most Critical Command | Key Technical Concept to State |
| :--- | :--- | :--- | :--- |
| **Kali Linux** | Debian-based security auditing OS | `ip a` | Environment is an OS in a VM; uses VirtualBox network adapters. |
| **WHOIS** | Domain registration lookup protocol | `whois <domain>` | Passive footprinting querying ICANN/RIR public registries. |
| **nslookup** | Command-line DNS query utility | `nslookup -type=mx <domain>` | Resolves A (IP), MX (Mail), NS (Nameserver), PTR (Reverse). |
| **Nmap** | Network discovery & port scanner | `sudo nmap -sS -sV -O <IP>` | SYN scan (`-sS`) is half-open; open port $\neq$ vulnerability. |
| **theHarvester** | OSINT email/subdomain collector | `theHarvester -d <dom> -b crtsh` | Scrapes public search engines & Certificate Transparency logs. |
| **Netdiscover** | Layer 2 local ARP scanner | `sudo netdiscover -r <subnet>` | Broadcasts ARP requests; strictly limited to local subnets. |
| **Sublist3r** | Search engine subdomain enumerator | `sublist3r -d <domain> -o out.txt` | Aggregates subdomains from search engine indexes passively. |
| **Nikto** | Active web server vulnerability scanner | `nikto -h <target> -p 443 -ssl` | Active HTTP checks; flags header disclosures & missing flags. |

---

### High-Probability Exam Viva Questions & Answers

1. **What is the difference between active and passive reconnaissance?**  
   *Answer:* Active reconnaissance (Nmap, Netdiscover, Nikto) directly interacts with target systems by transmitting probe packets, making it detectable by firewalls and IDSs. Passive reconnaissance (WHOIS, theHarvester, Sublist3r) queries third-party public databases without sending any packets to the target organization.
2. **Why does Nmap SYN scan (`-sS`) require root privileges while Connect scan (`-sT`) does not?**  
   *Answer:* `-sT` uses standard Berkeley sockets provided by the operating system, which handles the complete TCP handshake automatically. `-sS` requires raw sockets to manually construct and inject custom SYN packets and tear down connections with RST packets before completion.
3. **What is the purpose of the Address Resolution Protocol (ARP) in Netdiscover?**  
   *Answer:* ARP resolves Layer 3 IP addresses to Layer 2 physical MAC addresses within a local broadcast domain. Netdiscover uses ARP because all active devices on a subnet must respond to ARP requests to communicate on the local network.
4. **Why is an open port not necessarily an exploitable vulnerability?**  
   *Answer:* An open port merely indicates an accessible, listening network service. A vulnerability exists only if that specific service has a programming flaw, outdated version, or configuration error that can be exploited.
5. **How does Certificate Transparency improve subdomain enumeration?**  
   *Answer:* Certificate Transparency (CT) mandates that certificate authorities log every issued SSL/TLS certificate to publicly searchable append-only logs (`crt.sh`), allowing analysts to uncover valid subdomains that search engines have not yet indexed.
