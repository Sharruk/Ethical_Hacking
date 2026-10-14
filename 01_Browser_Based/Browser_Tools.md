# Ethical Hacking Laboratory: Browser-Based & Web-Interface Tools
**Course:** ICS1561 – Ethical Hacking Laboratory  
**Category:** 01 – Browser-Based and Web-Interface Tools  
**Target Environment:** Web Browser / Local Web Services  
**Reference Lab Manuals:** Week 4 (Google Dorking), Week 5 (Shodan), Week 7 (Nessus), Week 8 (Robtex)

---

## Table of Contents
1. [Tool 1: Robtex](#1-tool-1-robtex)
   - [A. Aim](#a-aim)
   - [B. What is the Tool?](#b-what-is-the-tool)
   - [C. Step-by-Step Procedure](#c-step-by-step-procedure)
   - [D. What to Observe](#d-what-to-observe)
   - [E. Observation — Write in the Lab Record](#e-observation--write-in-the-lab-record)
   - [F. Result](#f-result)
   - [G. Viva Questions](#g-viva-questions)
   - [H. Quick Revision](#h-quick-revision)
2. [Tool 2: Shodan](#2-tool-2-shodan)
   - [A. Aim](#a-aim-1)
   - [B. What is the Tool?](#b-what-is-the-tool-1)
   - [C. Step-by-Step Procedure](#c-step-by-step-procedure-1)
   - [D. What to Observe](#d-what-to-observe-1)
   - [E. Observation — Write in the Lab Record](#e-observation--write-in-the-lab-record-1)
   - [F. Result](#f-result-1)
   - [G. Viva Questions](#g-viva-questions-1)
   - [H. Quick Revision](#h-quick-revision-1)
3. [Tool 3: Google Dorking](#3-tool-3-google-dorking)
   - [A. Aim](#a-aim-2)
   - [B. What is the Tool?](#b-what-is-the-tool-2)
   - [C. Step-by-Step Procedure](#c-step-by-step-procedure-2)
   - [D. What to Observe](#d-what-to-observe-2)
   - [E. Observation — Write in the Lab Record](#e-observation--write-in-the-lab-record-2)
   - [F. Result](#f-result-2)
   - [G. Viva Questions](#g-viva-questions-2)
   - [H. Quick Revision](#h-quick-revision-2)
4. [Tool 4: Nessus Essentials](#4-tool-4-nessus-essentials)
   - [A. Aim](#a-aim-3)
   - [B. What is the Tool?](#b-what-is-the-tool-3)
   - [C. Step-by-Step Procedure](#c-step-by-step-procedure-3)
   - [D. What to Observe](#d-what-to-observe-3)
   - [E. Observation — Write in the Lab Record](#e-observation--write-in-the-lab-record-3)
   - [F. Result](#f-result-3)
   - [G. Viva Questions](#g-viva-questions-3)
   - [H. Quick Revision](#h-quick-revision-3)
   - [Exam Procedure Summary](#exam-procedure-summary)
5. [Overall Comparison Table](#5-overall-comparison-table)
6. [Final Practical Revision](#6-final-practical-revision)
   - [Click-by-Click Sequence for Each Tool](#click-by-click-sequence-for-each-tool)
   - [Most Important Terms](#most-important-terms)
   - [One-Minute Record Summary (Aim, Procedure, Observation, Result)](#one-minute-record-summary-aim-procedure-observation-result)
   - [One-Minute Viva Revision](#one-minute-viva-revision)

---

## 1. Tool 1: Robtex

### A. Aim
To perform passive domain footprinting and gather DNS and network intelligence for the target domain `ssn.edu.in` using the web-based Robtex tool.

### B. What is the Tool?
Robtex (`https://www.robtex.com`) is a free, web-based network and DNS intelligence tool that aggregates publicly available DNS, routing, and domain registration data into a single browsable interface. It provides visual graphs and structured tables showing relationships between a target domain, its name servers, mail exchangers, IP addresses, and neighbouring domains. Because Robtex queries its own accumulated database rather than interacting directly with the target infrastructure, it is a purely passive reconnaissance tool.

### C. Step-by-Step Procedure
1. **Open Robtex in a Browser:** Launch any standard web browser (Chrome, Firefox, or Edge) and navigate to `https://www.robtex.com`. No account registration or local software installation is required.
2. **Enter the Target Domain:** In the primary search bar on the homepage, type `ssn.edu.in` and press **Enter** or click the search icon.
3. **Select the DNS Result:** From the search suggestions and dropdown categories, select the **DNS** result for `ssn.edu.in` (do not select the Reputation result).
4. **Examine DNS Summary and Records:** Review the summary card displaying the domain's resolved IP address, hosting provider, authoritative name servers, and mail servers.
5. **Identify Key DNS Record Types:**
   - **A Records:** Review the IPv4 addresses mapped to the domain and its service hosts.
   - **MX Records:** Identify the mail exchangers handling incoming email for the organization.
   - **NS Records:** Identify the authoritative name servers delegated to serve the DNS zone.
   - **TXT Records:** Inspect text strings, noting Sender Policy Framework (SPF) records, DKIM public keys, and third-party verification tokens.
   - **PTR Records:** Perform reverse DNS verification to see which hostname is assigned to an observed IP.
   - **SOA Record:** Inspect the Start of Authority record detailing the primary name server, administrator email, zone serial number, and refresh/retry timers.
6. **Open Subdomains Section:** Scroll down to the **Subdomains** panel and inspect the list of discovered publicly indexed hostnames (such as `lms.ssn.edu.in`, `www.ssn.edu.in`, and `coe.ssn.edu.in`).
7. **Open DNS History:** Click on the **DNS History** tab and examine active vs. former records to see how the organization's infrastructure has evolved over time.
8. **Open DNS Trace:** Navigate to the **DNS Trace** tab to inspect the step-by-step root-to-authoritative delegation chain, the authoritative response, the DNSSEC validation status, query round-trip latency, and overall record counts.
9. **Use IP Lookup:** Click on any resolved IP address (or enter an IP directly into the search bar) to review its Autonomous System Number (ASN), IP network range (CIDR), ISP ownership, and co-hosted domains.

### D. What to Observe
- **A (Address) Record:** Maps a fully qualified domain name (FQDN) to a 32-bit IPv4 address (forward resolution).
- **MX (Mail Exchange) Record:** Specifies the mail servers responsible for accepting incoming email messages on behalf of the domain, including their priority values.
- **NS (Name Server) Record:** Specifies the authoritative DNS servers that hold the official DNS zone records for the domain.
- **TXT (Text) Record:** Carries human- or machine-readable text used for domain security policies, including SPF (`v=spf1 ...`) to prevent email spoofing, DKIM, and domain ownership validation.
- **PTR (Pointer) Record:** Maps an IPv4 or IPv6 address back to a canonical hostname within the `in-addr.arpa` zone (reverse DNS resolution).
- **SOA (Start of Authority) Record:** Declares core administrative information about the DNS zone, such as the primary master name server, contact email, serial number, and zone caching parameters.
- **DNS History:** Displays a timeline of DNS records indexed by Robtex over time, separating records currently active from former records that have been discontinued or changed.
- **DNS Trace:** Visualizes the hierarchical DNS delegation path from the DNS Root servers (`.`) to the Top-Level Domain servers (`.in`, `.edu.in`) down to the authoritative name servers, reporting query response times and DNSSEC status.
- **ASN (Autonomous System Number):** A globally unique identifier allocated to an autonomous network or ISP operating under a unified routing policy (e.g., Bharti Airtel, Vodafone Idea).

### E. Observation — Write in the Lab Record
> **Observation (ssn.edu.in Lookup):**  
> Passive DNS reconnaissance of `ssn.edu.in` was conducted using Robtex. The domain's authoritative name servers were observed as `pdns.ssn.edu.in`, `sdns.ssn.edu.in`, and `tdns.ssn.edu.in`, with `pdns.ssn.edu.in` resolving to the A record `182.75.25.254` (routed via Bharti Airtel / Vodafone Idea network blocks). Mail routing (MX) is outsourced to Google Workspace infrastructure (`aspmx.l.google.com` and alternate servers `alt1.aspmx.l.google.com` through `alt4.aspmx.l.google.com`), supported by an SPF TXT record referencing `_spf.google.com`. The Subdomains panel revealed publicly indexed hosts including `lms.ssn.edu.in`, `www.ssn.edu.in`, and `coe.ssn.edu.in`. In the DNS Trace view, the domain was reported as unsigned (DNSSEC not enabled / Insecure, no DS record in parent delegation), with an observed query response time of 482 ms. The DNS History panel recorded 12 total entries (8 active records and 4 former records observed at the time of access). Note that DNS History active/former counts and query response times reflect specific test-session observations rather than permanent values.

### F. Result
Passive domain footprinting and DNS intelligence gathering for `ssn.edu.in` were successfully completed using Robtex, identifying its authoritative name servers, mail exchangers, subdomains, network ASN routing, and DNSSEC status without transmitting traffic to the target servers.

### G. Viva Questions
1. **What is Robtex and why is it categorized as a passive reconnaissance tool?**  
   *Answer:* Robtex is a web-based network and DNS intelligence search tool. It is passive because it gathers data from its own pre-aggregated public database of DNS records, BGP routing tables, and historical scans without sending probe packets directly to the target organization's servers.
2. **What is the functional difference between an A record and a PTR record?**  
   *Answer:* An A record performs forward resolution by mapping a hostname to an IPv4 address, whereas a PTR record performs reverse resolution by mapping an IP address back to its associated hostname.
3. **What is an Autonomous System Number (ASN) and why is it valuable in footprinting?**  
   *Answer:* An ASN is a globally unique number assigned to an autonomous routing network managed by an ISP or enterprise. In footprinting, identifying the ASN reveals the target's hosting provider, IP address ranges, and network infrastructure boundaries.
4. **Does the presence of a former record in DNS History indicate that the service is currently vulnerable?**  
   *Answer:* No. Former records represent historical DNS entries observed in the past that may have since been decommissioned, migrated, or updated. They serve as historical intelligence leads and must be verified against current live configurations.
5. **What does an "unsigned" DNSSEC status mean in a DNS Trace report?**  
   *Answer:* It means the domain has not deployed DNS Security Extensions (DNSSEC) with cryptographic signatures and Delegation Signer (DS) records in the parent zone, leaving client DNS queries theoretically open to DNS spoofing or cache poisoning attacks.

### H. Quick Revision
- **Robtex:** Free browser-based DNS and BGP routing intelligence tool (`robtex.com`).
- **A Record:** Hostname $\rightarrow$ IPv4 address.
- **MX Record:** Mail server locator with assigned priorities.
- **NS Record:** Authoritative DNS server for the zone.
- **TXT Record:** Holds SPF (`v=spf1`), DKIM, and ownership verification strings.
- **PTR Record:** IPv4 address $\rightarrow$ Canonical hostname (reverse DNS).
- **SOA Record:** Zone authority, serial number, and zone transfer timers.
- **DNS History:** Chronological log of active vs. decommissioned records.
- **DNS Trace:** Root-to-authoritative DNS hierarchy, response timing, and DNSSEC verification.
- **ASN:** Identifies the ISP or autonomous network routing the target IP block.

---

## 2. Tool 2: Shodan

### A. Aim
To perform passive internet-wide device reconnaissance, discover open network ports, and inspect publicly observed service banners for the target IP `8.8.8.8` using Shodan.

### B. What is the Tool?
Shodan (`https://www.shodan.io`) is a specialized search engine that continuously scans the entire public IPv4 address space to index publicly exposed, internet-connected devices such as web servers, routers, firewalls, and IoT devices. Rather than crawling web page textual content like traditional search engines, Shodan interacts with listening network ports and captures the raw service banners returned by services. Querying Shodan constitutes passive reconnaissance because users query Shodan's cached scan database rather than directly probing the target host.

### C. Step-by-Step Procedure
1. **Open Shodan in a Browser:** Launch any standard web browser and navigate to `https://www.shodan.io`.
2. **Enter the Target IP Address:** In the main search bar, type `8.8.8.8` (the lab manual's standard example IP address representing Google Public DNS).
3. **Initiate the Search:** Press **Enter** or click the search button.
4. **Examine General Host Overview:** Review the left-hand panel showing the host's primary metadata: IP address, reverse DNS hostname, operating organization / Autonomous System, ISP, and geographic location (country, city).
5. **Locate the Open Ports Section:** Review the list of open TCP and UDP ports detected and indexed by Shodan's scanning engines.
6. **Inspect Associated Services and Banners:** For each detected port, examine the identified service protocol (e.g., DNS on port 53, HTTPS on port 443) and read the associated service banner containing protocol version headers, TLS certificates, and server identification strings.
7. **Record Findings:** Document the observed hostname, organization, country, open ports, and service banners.
8. **Assess Information Currency:** Keep in mind that Shodan displays previously collected intelligence; the results reflect the state of the target at the time of Shodan's last crawler scan and may differ from its instantaneous live state.

### D. What to Observe
- **IP Address:** The unique 32-bit numerical identifier assigned to the host on the public internet (`8.8.8.8`).
- **Hostname:** The domain name associated with the IP address discovered through reverse DNS lookups (e.g., `dns.google`).
- **Open Port:** A communication endpoint (port number) on the target host that is actively accepting network connections.
- **Service:** The application-layer network protocol or daemon operating on an open port (e.g., DNS, HTTP, HTTPS, SSH).
- **Service Banner:** The raw textual response, greeting banner, or HTTP header sent by an open service upon connection, which often discloses server daemon software, version numbers, and operating system attributes.
- **Organisation:** The registered enterprise, ISP, or cloud provider that owns the Autonomous System and IP address range (e.g., Google LLC).
- **Location:** The physical or registered geographic location of the host, including country, city, and approximate geographical coordinates.

### E. Observation — Write in the Lab Record
> **Observation (Shodan Host Lookup):**  
> A passive device and service search was performed on Shodan for the IP address `8.8.8.8`. The host summary identified the reverse hostname as `dns.google`, the organization as `Google LLC`, and the geographical location as `United States`. The Open Ports section displayed listening services on ports `[observed open ports, e.g., 53, 443]`, corresponding to services `[observed services, e.g., DNS, HTTPS]`. Associated service banners displayed protocol metadata and server header responses. Because Shodan retrieves information gathered by its distributed scanners over time, the observed results represent Shodan's indexed repository snapshot and may not reflect the target's current live state.

### F. Result
Passive reconnaissance of the target IP address `8.8.8.8` was successfully conducted using Shodan, identifying its registered organization, reverse hostname, open network ports, and service banners without transmitting active packets to the target.

### G. Viva Questions
1. **What is Shodan and how does it fundamentally differ from Google?**  
   *Answer:* Shodan is a search engine for internet-connected devices, servers, and industrial systems that indexes open network ports and service banners. Google is a content search engine that crawls and indexes hyperlinked World Wide Web content (HTML, text, documents).
2. **Why is looking up an IP address on Shodan considered passive reconnaissance?**  
   *Answer:* Because the analyst only interacts with Shodan's central database of pre-indexed crawler results. No network traffic, port probes, or packets originate from the analyst's computer to the target host.
3. **What is a service banner and why is it critical during security assessments?**  
   *Answer:* A service banner is the introductory text or header emitted by a network service upon receiving a connection. It often reveals the exact software name, version number, and OS platform, enabling security analysts to identify known CVE vulnerabilities without intrusive exploitation.
4. **Why might Shodan's results differ from an active Nmap scan performed today?**  
   *Answer:* Shodan displays data collected during its automated crawlers' last visit, which may have occurred days or weeks ago. Recent firewall rules, port changes, service reconfigurations, or software updates will not appear until Shodan rescans the IP.
5. **What are common search filters used on Shodan to narrow queries?**  
   *Answer:* Common filters include `ip:` (specific IP address), `port:` (specific port number), `org:` (specific organization), `country:` (two-letter country code), and `product:` (specific software name).

### H. Quick Revision
- **Shodan:** Search engine for internet-connected devices, servers, and IoT (`shodan.io`).
- **Service Banner:** Text header emitted by an open service disclosing software brand and version.
- **Open Port:** Listening TCP/UDP network communication endpoint.
- **Hostname:** Reverse DNS name resolved for an IP (`dns.google`).
- **Organization / ISP:** Registered owner of the network and IP block.
- **Passive Nature:** Relies entirely on Shodan's crawler index; zero direct probes to target.
- **Data Latency:** Data reflects the last crawler scan timestamp, not necessarily real-time state.

---

## 3. Tool 3: Google Dorking

### A. Aim
To perform passive reconnaissance on public domain documentation using advanced Google search operators (Google Dorking) to isolate specific indexed file formats, page titles, and URL structures without contacting the target web server.

### B. What is the Tool?
Google Dorking (also known as Google Hacking) is a passive reconnaissance technique that uses advanced search operators to filter and query Google's massive index of public web content. By structuring specific search parameters, security analysts can locate publicly exposed documentation, unlinked administration portals, configuration guides, and specific file types that web crawlers have indexed. Because queries are executed strictly against Google's search index servers, no packets or HTTP requests are sent to the target organization's infrastructure.

### C. Step-by-Step Procedure
1. **Open Google in a Browser:** Launch any standard web browser and open `https://www.google.com`.
2. **Execute Domain Restriction Query:** In the search input box, enter the domain operator:
   ```text
   site:owasp.org
   ```
   Press **Enter** and review the results, confirming that every returned web link belongs strictly to `owasp.org` or its subdomains.
3. **Search for Specific Indexed Documents:** Enter the combined file type and keyword query:
   ```text
   filetype:pdf site:owasp.org cybersecurity
   ```
   Press **Enter** and examine the results, observing that Google filters the output to only publicly indexed PDF documents containing the term "cybersecurity" on `owasp.org`.
4. **Search for Specific Page Titles:** Enter the title restriction query:
   ```text
   intitle:"cheat sheet" site:owasp.org
   ```
   Press **Enter** and verify that all matching search results contain the exact phrase "cheat sheet" within their HTML `<title>` tags.
5. **Search for Specific URL Strings:** Enter the URL path operator:
   ```text
   inurl:testing-guide site:owasp.org
   ```
   Press **Enter** and verify that returned web pages contain the substring `testing-guide` in their URL paths.
6. **Experiment with Additional Advanced Operators:**
   - Execute `intext:"vulnerability" site:owasp.org` to search for words appearing within the visible page body text.
   - Execute `site:owasp.org -inurl:blog` to exclude blog posts from the search results.
   - Execute `"threat modeling" site:owasp.org` to enforce an exact phrase match.
   - Execute `site:owasp.org (filetype:pdf OR filetype:pptx)` to match either PDF or PowerPoint documents.
7. **Document Findings Ethically:** Record the types and structure of public documentation discovered. Do not use search operators to hunt for real credentials, sensitive personal information, or exposed third-party configuration files.

### D. What to Observe
#### Practical Queries and Explanations
| Query | Explanation |
| :--- | :--- |
| `site:owasp.org` | Restricts search results strictly to the designated domain (`owasp.org`) and its subdomains, filtering out all other websites. |
| `filetype:pdf site:owasp.org cybersecurity` | Filters indexed results to return only PDF documents hosted on `owasp.org` that contain the keyword "cybersecurity". |
| `intitle:"cheat sheet" site:owasp.org` | Restricts results to pages where the exact phrase "cheat sheet" appears inside the HTML document `<title>` tag on `owasp.org`. |
| `inurl:testing-guide site:owasp.org` | Restricts results to pages where the URL string itself contains the specific directory or file path keyword "testing-guide". |

#### Additional Search Operators
- `intext:` — Searches for the specified keyword or phrase specifically within the visible body text of the web page, ignoring titles and URLs.
- `-` (Minus Operator) — Excludes search results that contain the designated word, phrase, or operator parameter (e.g., `site:owasp.org -inurl:blog`).
- `" "` (Double Quotes) — Forces Google to search for the exact phrase in the precise word sequence specified without synonym substitution.
- `OR` (or `|`) — Logical disjunction operator that returns results matching either the left operand or the right operand (e.g., `filetype:pdf OR filetype:pptx`).

### E. Observation — Write in the Lab Record
> **Observation (Google Dorking on Public Scope):**  
> Advanced Google search operators were applied to the public educational domain `owasp.org` to perform passive reconnaissance without generating traffic against the target's servers. The `site:owasp.org` query successfully restricted all results to the target domain. Combining this with `filetype:pdf` and the keyword `cybersecurity` filtered results to return only publicly indexed technical PDF files. The query `intitle:"cheat sheet"` isolated specific Cheat Sheet series pages containing the phrase in their HTML title tags, and `inurl:testing-guide` isolated the Web Security Testing Guide URL paths. All lookups were processed entirely against Google's search engine index, ensuring zero direct engagement with the target organization.

### F. Result
Passive reconnaissance using advanced Google search operators was successfully demonstrated on public documentation, isolating targeted file types, document titles, and URL paths without sending network traffic to the target web server.

### G. Viva Questions
1. **What is Google Dorking and why is it considered a passive footprinting technique?**  
   *Answer:* Google Dorking is the use of advanced search operators to filter Google's web index for specialized information. It is passive because all queries are processed solely against Google's cached index; no requests or probe packets are sent to the target web server.
2. **What is the difference between the `site:` operator and the `inurl:` operator?**  
   *Answer:* `site:` restricts search results to a specific domain or host (e.g., `site:owasp.org`), whereas `inurl:` searches for specific keywords or file paths located anywhere within the URL string (e.g., `inurl:login`).
3. **How does the `filetype:` operator assist an ethical hacker during reconnaissance?**  
   *Answer:* It enables testers to discover publicly accessible files of specific formats (such as `.pdf`, `.docx`, `.xlsx`, `.conf`, or `.sql`) that may contain architectural diagrams, policy documents, employee lists, or accidental metadata disclosures.
4. **How does the exclusion operator (`-`) improve search precision?**  
   *Answer:* Placing a minus sign immediately before a term or operator excludes all pages containing that term, allowing the tester to eliminate noise (e.g., excluding common forum or blog pages with `-inurl:forum`).
5. **What ethical and legal boundaries must be respected when practicing Google Dorking?**  
   *Answer:* Queries must be restricted to authorized targets, educational research, and legitimate public documentation. Testers must never use dorks to search for, download, or exploit private user records, unauthorized credentials, financial data, or exposed administrative portals belonging to third parties.

### H. Quick Revision
- **Google Dorking:** Querying Google's search index using specialized operators.
- `site:`: Limits results to a specific domain (`site:domain.com`).
- `filetype:`: Limits results to a specific file extension (pdf, docx, xlsx, conf).
- `intitle:`: Matches keywords inside the HTML `<title>` tag.
- `inurl:`: Matches keywords inside the web page URL path.
- `intext:`: Matches keywords inside the visible page body text.
- `-` (Minus): Excludes pages containing the specified term or filter.
- `" "` (Quotes): Enforces strict, exact-phrase matching.
- `OR`: Matches either of the alternate query terms.
- **Ethical Rule:** Zero probing of third-party confidential data or exposed credentials.

---

## 4. Tool 4: Nessus Essentials

### A. Aim
To perform an automated vulnerability assessment on an authorized target host using Nessus Essentials, categorize identified security weaknesses by severity, and evaluate associated CVE references and remediation measures.

### B. What is the Tool?
Nessus Essentials is a free, entry-level automated vulnerability scanner developed by Tenable that allows security professionals and students to audit up to 16 IP addresses. It operates by scanning target systems across thousands of modular vulnerability check scripts known as plugins, evaluating network services for missing security patches, dangerous default configurations, weak credentials, and compliance issues. Nessus runs locally as a background operating system service and is accessed and controlled via a web browser interface at `https://localhost:8834`.

### C. Step-by-Step Procedure
1. **Open Nessus in the Browser:** Verify that the Tenable Nessus service is running locally (`net start "Tenable Nessus"` on Windows or `systemctl status nessusd` on Linux). Launch a web browser and navigate to `https://localhost:8834`.
2. **Accept Certificate Warning:** If the browser displays a self-signed SSL certificate warning, click **Advanced** and select **Proceed to localhost (unsafe)** to open the login portal.
3. **Sign In:** On the login screen, enter the authorized lab credentials provided for your instance and click **Sign In**. *(Never commit or write down passwords).*
4. **Open My Scans:** From the left-hand navigation sidebar under the **Scans** heading, click on **My Scans**.
5. **Create a New Scan:** In the upper-right corner of the interface, click the blue **New Scan** button.
6. **Select Scan Template:** From the library of scan templates, select **Basic Network Scan** (a comprehensive template suitable for scanning any system).
7. **Configure Scan Settings:**
   - In the **Name** field, enter a descriptive label such as `Lab Vulnerability Assessment`.
   - Set the **Folder** destination to `My Scans`.
   - In the **Targets** field, enter the confirmed IPv4 address of an authorized lab virtual machine (e.g., a local DVWA or Metasploitable VM). *Never enter unauthorized or external public IP addresses.*
8. **Save Scan Configuration:** Click the blue **Save** button at the bottom of the page to store the configured scan under My Scans.
9. **Launch the Scan:** In the My Scans table, locate your saved scan and click the green **Play / Launch** icon on the right to start scanning.
10. **Monitor Scan Progress:** Observe the rotating status indicator and wait for the scan to finish execution (a green checkmark and completion status will appear).
11. **Open Completed Scan:** Click on the name of the finished scan to open its detailed reporting dashboard.
12. **Inspect Hosts and Vulnerabilities Tabs:**
    - Under the **Hosts** tab, verify the target IP address and overall vulnerability distribution bar chart.
    - Click the **Vulnerabilities** tab to review the complete list of identified findings sorted by severity.
13. **Examine Severity Distribution:** Review the findings categorized across the five Nessus severity tiers: Critical, High, Medium, Low, and Informational.
14. **Inspect a Detailed Finding:** Click on an identified vulnerability (prioritizing Critical or High findings) to inspect:
    - **Description:** Summary of the vulnerability and security implications.
    - **Evidence / Plugin Output:** The technical proof captured by Nessus during the probe.
    - **Solution:** The official vendor patch or configuration fix.
    - **Plugin Information:** Plugin ID, family, publication date, and CVSS v3.0/v2.0 base scores.
    - **See Also / References:** Hyperlinks to vendor advisories and standardized CVE numbers.
15. **Research Reported CVEs:** Note any associated CVE identifier (format: `CVE-YYYY-NNNNN`) and open `https://www.cve.org` or the NIST National Vulnerability Database (`https://nvd.nist.gov`) in a new browser tab to inspect its Common Vulnerability Scoring System (CVSS) attack vector metrics, impact scores, and patch status.
16. **Export Report (If Supported):** In the top-right corner of the scan results dashboard, click the **Export** button and choose a report format (such as HTML, PDF, or CSV) to save the findings for documentation.

### D. What to Observe
#### Core Terminology
- **Vulnerability Assessment (VA):** The automated, systematic process of identifying, cataloguing, and prioritizing security flaws and misconfigurations in computing assets without actively weaponizing or exploiting them.
- **Basic Network Scan:** A full-system Nessus scanning template that inspects open ports, enumerates active services, queries operating system versions, and audits software against known vulnerability databases.
- **Nessus Plugin:** An individual, modular test script written in Tenable's NASL (Nessus Attack Scripting Language) designed to check for one specific vulnerability, software version, or configuration flaw.
- **CVE (Common Vulnerabilities and Exposures):** A globally standardized, unique alphanumeric reference identifier (e.g., `CVE-2021-34527`) assigned to a publicly known software vulnerability.
- **Remediation:** The prescribed mitigation action—such as applying a vendor software patch, updating firmware, disabling a vulnerable cipher, or modifying firewall rules—to eliminate an identified risk.
- **False Positive:** An incorrect report where a vulnerability scanner flags a system as vulnerable when the flaw does not actually exist or is rendered non-exploitable by compensating controls.

#### Severity Levels Table
| Severity Level | Color Code | CVSS Base Score | Practical Significance |
| :--- | :--- | :--- | :--- |
| **Critical** | Red | 9.0 – 10.0 | Extremely serious potential risk; enables unauthenticated remote code execution (RCE), full system takeover, or complete loss of system confidentiality and integrity. Requires immediate mitigation. |
| **High** | Orange | 7.0 – 8.9 | Significant security risk; enables unauthorized data access, privilege escalation, or substantial service disruption with minimal user interaction. |
| **Medium** | Yellow | 4.0 – 6.9 | Moderate risk; typically requires specific preconditions, local network access, or specialized user interaction to exploit successfully. |
| **Low** | Green | 0.1 – 3.9 | Lower-severity issue; includes minor protocol weaknesses, information disclosure, or configuration discrepancies with low practical exploitability. |
| **Info** | Blue | 0.0 (None) | Informational observation; non-vulnerable contextual findings gathered during host discovery, such as open port lists, MAC addresses, and web server software banners. |

### E. Observation — Write in the Lab Record
> **Observation (Nessus Assessment):**  
> An automated vulnerability assessment was conducted using Nessus Essentials against the authorized lab target IP `[observed target IP address, e.g., 192.168.56.102]`. The Basic Network Scan completed successfully and reported a total of `[observed total count]` findings categorized into `[observed Critical count]` Critical, `[observed High count]` High, `[observed Medium count]` Medium, `[observed Low count]` Low, and `[observed Info count]` Informational items. Analysis of the finding `[observed finding name]` (Plugin ID `[observed Plugin ID]`) revealed vulnerability details and remediation steps. The associated identifier `[observed CVE ID, e.g., CVE-YYYY-NNNNN]` was cross-referenced in the National Vulnerability Database (NVD) to review its CVSS score and attack vector metrics.

### F. Result
An automated vulnerability assessment was successfully performed on the authorized lab host using Nessus Essentials, categorizing detected security flaws by severity, evaluating CVE references, and generating remediation guidance.

### G. Viva Questions
1. **What is Nessus?**  
   *Answer:* Nessus is a leading commercial automated vulnerability scanner developed by Tenable that audits computers, servers, and network devices for known software vulnerabilities, missing patches, default passwords, and configuration errors.
2. **What is Nessus Essentials?**  
   *Answer:* Nessus Essentials is the free, non-commercial edition of Nessus provided by Tenable. It offers complete vulnerability scanning capabilities for up to 16 IP addresses per installation, making it ideal for educational and lab training.
3. **What is a Nessus plugin?**  
   *Answer:* A Nessus plugin is an independent script written in NASL (Nessus Attack Scripting Language) that tests for a specific vulnerability, version flaw, or misconfiguration. Nessus automatically downloads and compiles thousands of plugins to keep its checks current.
4. **What is a CVE?**  
   *Answer:* CVE stands for Common Vulnerabilities and Exposures. It is a globally standardized dictionary of unique reference numbers (e.g., `CVE-2021-34527`) maintained by MITRE to ensure security researchers and tools refer to the same specific vulnerability unambiguously.
5. **What is the difference between Nmap and Nessus?**  
   *Answer:* Nmap is primarily an active network discovery and port scanner used to discover live hosts, open ports, and running service versions. Nessus is a vulnerability assessment platform that goes further by testing those open services against thousands of plugin checks, scoring vulnerability severity with CVSS, and providing structured remediation solutions.

### H. Quick Revision
- **Nessus Essentials:** Free vulnerability assessment tool for up to 16 target IP addresses.
- **Local Access URL:** `https://localhost:8834` (requires local Tenable Nessus service to be running).
- **Basic Network Scan:** General-purpose network vulnerability scanning template.
- **Plugin:** Scripted test checking for a specific vulnerability or configuration flaw.
- **CVE:** Standardized vulnerability identifier (`CVE-YYYY-NNNNN`).
- **Severity Hierarchy:** Critical (Red), High (Orange), Medium (Yellow), Low (Green), Info (Blue).
- **False Positive:** Inaccurately reported vulnerability that does not actually exist on the target.
- **Target Policy:** Scan only confirmed, authorized lab machines (DVWA, Metasploitable); never external networks.

### Exam Procedure Summary
For rapid execution in a practical examination:
1. **Login:** Start the Tenable Nessus service, open `https://localhost:8834`, accept the self-signed SSL certificate warning, and sign in using the provided lab credentials.
2. **Scan Creation:** Navigate to **My Scans** $\rightarrow$ **New Scan**, and select the **Basic Network Scan** template.
3. **Target Configuration:** Enter a descriptive scan name (e.g., `Exam_VA_Scan`), keep the folder as `My Scans`, and enter the authorized target lab VM IP in the **Targets** field; click **Save**.
4. **Launch:** Under My Scans, click the green **Play/Launch** button next to the saved scan and await scan completion.
5. **Result Analysis:** Open the finished scan, inspect the severity graph, review Critical/High findings, record plugin output and CVE identifiers, and look up CVEs on `cve.org` or `nvd.nist.gov`.
6. **Report Export:** Click the **Export** button in the top-right corner to save an HTML or PDF report if requested by the examiner.

---

## 5. Overall Comparison Table

| Tool | Purpose | Main Steps | Main Output |
| :--- | :--- | :--- | :--- |
| **Robtex** | Passive DNS footprinting and network intelligence gathering. | 1. Navigate to `robtex.com`.<br>2. Enter domain (`ssn.edu.in`).<br>3. Inspect DNS records (A, MX, NS, TXT).<br>4. Review Subdomains, DNS History, and DNS Trace. | Forward/reverse DNS records, mail servers, subdomains, DNSSEC status, delegation chain, response latency, and ASN details. |
| **Shodan** | Search engine for publicly observed internet-connected devices and open ports. | 1. Navigate to `shodan.io`.<br>2. Search target IP (`8.8.8.8`).<br>3. Review host overview panel.<br>4. Inspect Open Ports and service banners. | Reverse hostname, organization, geographic location, open ports, service names, and raw service banners. |
| **Google Dorking** | Passive search engine reconnaissance using advanced operators on public index. | 1. Navigate to `google.com`.<br>2. Execute queries using `site:`, `filetype:`, `intitle:`, `inurl:`.<br>3. Review filtered pages and documents. | Publicly indexed PDF/office documents, specific page titles, hidden directories, and exposed URL structures. |
| **Nessus Essentials** | Automated vulnerability assessment and security auditing of network hosts. | 1. Sign in at `https://localhost:8834`.<br>2. Create New Scan $\rightarrow$ Basic Network Scan.<br>3. Enter authorized lab target IP and launch.<br>4. Analyze vulnerabilities and CVEs; export report. | Categorized vulnerability findings (Critical to Info), CVSS scores, plugin outputs, CVE numbers, and remediation solutions. |

---

## 6. Final Practical Revision

### Click-by-Click Sequence for Each Tool

```text
========================================================================================
ROBTEX CLICK-BY-CLICK
Browser -> robtex.com -> Search "ssn.edu.in" -> Select DNS Result -> Review Summary ->
Inspect A, MX, NS, TXT, PTR, SOA -> Scroll to Subdomains -> Open DNS History ->
Open DNS Trace (Check DNSSEC & Latency) -> Click IP for ASN Details
========================================================================================
SHODAN CLICK-BY-CLICK
Browser -> shodan.io -> Search "8.8.8.8" -> Press Enter -> Review Left Panel (Org, Country)
-> Scroll to Open Ports -> Inspect Listening Services -> Read Raw Service Banners ->
Note Hostname and ISP Details
========================================================================================
GOOGLE DORKING CLICK-BY-CLICK
Browser -> google.com -> Enter site:owasp.org -> Enter filetype:pdf site:owasp.org cybersecurity
-> Enter intitle:"cheat sheet" site:owasp.org -> Enter inurl:testing-guide site:owasp.org ->
Test intext:, -, " ", OR -> Inspect Returned Public Documents
========================================================================================
NESSUS ESSENTIALS CLICK-BY-CLICK
Browser -> https://localhost:8834 -> Accept SSL Warning -> Enter Lab Credentials -> Sign In
-> My Scans -> New Scan -> Basic Network Scan -> Name: Lab_VA -> Targets: [Target IP] ->
Save -> Click Play (Launch) -> Wait for Completion -> Open Scan -> Review Vulnerabilities Tab
-> Click Critical/High Finding -> Inspect Description & CVE -> Cross-Reference on cve.org ->
Export Report
========================================================================================
```

### Most Important Terms
- **Passive Footprinting:** Gathering target intelligence from third-party public databases without directly interacting with the target's network.
- **A Record:** Maps a hostname to an IPv4 address.
- **MX Record:** Maps a domain to its responsible mail server(s).
- **NS Record:** Maps a domain to its authoritative name server(s).
- **TXT Record:** Stores arbitrary text, primarily SPF (`v=spf1 ...`) and DKIM for email security and verification.
- **PTR Record:** Maps an IPv4 address to its reverse DNS canonical hostname.
- **SOA Record:** Contains zone authority parameters, serial number, and zone transfer timers.
- **ASN:** Autonomous System Number identifying the network and ISP routing an IP block.
- **DNSSEC:** Cryptographic extension authenticating DNS responses (unsigned = no DS record in parent).
- **Shodan:** Device search engine indexing listening network ports and service banners.
- **Service Banner:** Text header emitted by an active service disclosing software and version.
- **Google Dorking:** Advanced Google queries using operators (`site:`, `filetype:`, `intitle:`, `inurl:`, `intext:`, `-`, `" "`, `OR`).
- **Vulnerability Assessment:** Systematic identification, classification, and scoring of security flaws.
- **Nessus Plugin:** Scripted test checking for a specific vulnerability or misconfiguration.
- **CVE:** Common Vulnerabilities and Exposures standardized naming dictionary (`CVE-YYYY-NNNNN`).
- **CVSS:** Common Vulnerability Scoring System quantifying vulnerability severity (0.0 to 10.0).
- **False Positive:** Scanner flagging a non-existent or unexploitable vulnerability.

### One-Minute Record Summary (Aim, Procedure, Observation, Result)

#### Tool 1: Robtex
- **Aim:** To perform passive DNS footprinting and network intelligence gathering on `ssn.edu.in` using Robtex.
- **Procedure:** Open `robtex.com`, search `ssn.edu.in`, select the DNS result, inspect A, MX, NS, TXT records, subdomains, DNS history, and DNS trace.
- **Observation:** Authoritative NS are `pdns.ssn.edu.in`, `sdns.ssn.edu.in`, `tdns.ssn.edu.in` (`pdns` resolves to `182.75.25.254`); MX handled by Google Workspace (`aspmx.l.google.com`); subdomains include `lms`, `www`, `coe`; DNSSEC is unsigned/insecure; DNS history showed 12 records (8 active, 4 former); DNS trace response latency was 482 ms.
- **Result:** Passive DNS records, mail servers, subdomains, and network ASN details were successfully gathered without directly contacting the target.

#### Tool 2: Shodan
- **Aim:** To perform passive device and service banner intelligence gathering for IP `8.8.8.8` using Shodan.
- **Procedure:** Open `shodan.io`, search IP `8.8.8.8`, review host metadata, inspect open ports, and examine service banners.
- **Observation:** Target `8.8.8.8` resolved to hostname `dns.google`, organization `Google LLC`, located in the United States, displaying listening services on ports `[observed open ports]` and associated service banners from Shodan's indexed database.
- **Result:** Publicly observed host metadata, open ports, and service banners for the target IP were successfully retrieved from Shodan's database.

#### Tool 3: Google Dorking
- **Aim:** To perform passive reconnaissance using advanced Google search operators on public domain documentation.
- **Procedure:** Open `google.com`, execute queries using `site:`, `filetype:`, `intitle:`, `inurl:`, and `intext:` operators against `owasp.org`, and inspect returned documents.
- **Observation:** Advanced operators filtered Google's search index without contacting target servers; `site:owasp.org` restricted results to the domain, `filetype:pdf` located public technical documents, `intitle:` matched cheat sheets, and `inurl:` filtered guide directories.
- **Result:** Publicly indexed technical documents, guides, and pages were successfully isolated using advanced Google search operators.

#### Tool 4: Nessus Essentials
- **Aim:** To perform an automated vulnerability assessment on an authorized lab target host using Nessus Essentials.
- **Procedure:** Open `https://localhost:8834`, sign in, configure a Basic Network Scan with the authorized target IP, launch the scan, wait for completion, and analyze findings by severity and CVE.
- **Observation:** Basic Network Scan on target `[observed target IP]` identified vulnerabilities categorized into Critical `[count]`, High `[count]`, Medium `[count]`, Low `[count]`, and Info `[count]`; inspected finding `[observed finding]` and cross-referenced CVE `[observed CVE]` on NVD.
- **Result:** Automated vulnerability assessment completed successfully, categorizing security weaknesses by severity, identifying CVEs, and documenting remediation guidance.

### One-Minute Viva Revision
- **Q: What is the main difference between active and passive reconnaissance?**  
  *A:* Active reconnaissance directly interacts with the target (transmitting packets/probes that can be logged by firewalls/IDS). Passive reconnaissance gathers information strictly from third-party public databases (Robtex, Shodan, Google) without sending any packets to the target.
- **Q: What does an SPF TXT record do?**  
  *A:* It specifies which mail servers and IP addresses are authorized to send email on behalf of the domain (`v=spf1 ...`), preventing email address spoofing.
- **Q: Why might Shodan show an open port that Nmap reports as closed?**  
  *A:* Shodan displays cached historical data from its last crawler run; if the host was reconfigured, firewalled, or shut down after that scan, Shodan's record will be out of date until rescanned.
- **Q: What operator would you use to find only Word documents on a specific university domain?**  
  *A:* `site:example.edu (filetype:doc OR filetype:docx)`
- **Q: What are the five severity levels in Nessus?**  
  *A:* Critical (9.0–10.0), High (7.0–8.9), Medium (4.0–6.9), Low (0.1–3.9), and Informational (0.0).
- **Q: How does Nessus determine if a system is vulnerable?**  
  *A:* It runs individual scripts called plugins that query network banners, probe specific endpoints, check software version strings, or inspect missing security patches against Tenable's vulnerability database.
