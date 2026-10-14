# Windows Ethical Hacking Laboratory: Windows Tools Practical Notes
**Course:** ICS1561 – Ethical Hacking Laboratory  
**Category:** 02 – Windows and GUI Tools / 01 – Windows Tools  
**Target Scope:** FOCA, Maltego Community Edition, Microsoft Sysmon  
**Reference Lab Manuals:** Week 4 (FOCA Installation & Metadata), Week 5 (Maltego & Shodan), Week 2 & Reconnaissance Tools (Sysmon)

---

## Table of Contents
1. [Tool 1: FOCA (Document Metadata Harvesting & OSINT)](#1-tool-1-foca-document-metadata-harvesting--osint)
   - [Aim](#aim)
   - [Introduction](#introduction)
   - [Requirements](#requirements)
   - [Step-by-Step Procedure](#step-by-step-procedure)
   - [What to Observe](#what-to-observe)
   - [Observation for the Lab Record](#observation-for-the-lab-record)
   - [Result](#result)
   - [Viva Questions and Answers](#viva-questions-and-answers)
   - [Quick Revision](#quick-revision)
2. [Tool 2: Maltego (Visual Link Analysis & OSINT Infrastructure Mapping)](#2-tool-2-maltego-visual-link-analysis--osint-infrastructure-mapping)
   - [Aim](#aim-1)
   - [Introduction](#introduction-1)
   - [Requirements](#requirements-1)
   - [Step-by-Step Procedure](#step-by-step-procedure-1)
   - [What to Observe](#what-to-observe-1)
   - [Observation for the Lab Record](#observation-for-the-lab-record-1)
   - [Result](#result-1)
   - [Viva Questions and Answers](#viva-questions-and-answers-1)
   - [Quick Revision](#quick-revision-1)
3. [Tool 3: Sysmon (System Monitor & Endpoint Activity Logging)](#3-tool-3-sysmon-system-monitor--endpoint-activity-logging)
   - [Aim](#aim-2)
   - [Introduction](#introduction-2)
   - [Requirements](#requirements-2)
   - [Step-by-Step Procedure](#step-by-step-procedure-2)
   - [What to Observe](#what-to-observe-2)
   - [Observation for the Lab Record](#observation-for-the-lab-record-2)
   - [Result](#result-2)
   - [Viva Questions and Answers](#viva-questions-and-answers-2)
   - [Quick Revision](#quick-revision-2)
4. [Combined Revision Section](#4-combined-revision-section)
   - [Overall Tool Comparison Matrix](#overall-tool-comparison-matrix)
   - [High-Level Functional Distinction](#high-level-functional-distinction)
   - [One-Minute Record Summary (Aim, Procedure, Observation, Result)](#one-minute-record-summary-aim-procedure-observation-result)

---

## 1. Tool 1: FOCA (Document Metadata Harvesting & OSINT)

### Aim
To discover publicly available documents associated with a target domain (`owasp.org`) and extract hidden metadata to identify server folders, software versions, usernames, and infrastructure details.

### Introduction
FOCA (Fingerprinting Organizations with Collected Archives) is an Open Source Intelligence (OSINT) tool developed to automate the discovery and analysis of public documents published on websites. It queries external search engines to locate files such as PDF, DOCX, XLSX, and PPTX, downloads them, and extracts embedded metadata (EXIF, author properties, software footprints, and internal server paths). Because initial discovery relies strictly on indexed search-engine caches, FOCA functions as a passive reconnaissance tool.

- **Official Repository:** [https://github.com/ElevenPaths/FOCA/releases](https://github.com/ElevenPaths/FOCA/releases)
- **Lab Execution Version:** FOCA v3.4.7.1 on Windows.

### Requirements
- **Operating System:** Windows 10 / 11 or Windows Server (x64).
- **Prerequisite Database Engine:** Microsoft SQL Server Express (2014, 2017, or 2019) or SQL Server LocalDB, required by FOCA for project data storage.
- **Runtime Environment:** Microsoft .NET Framework 4.7.2 or higher.
- **Application Binary:** FOCA executable (`FOCA.exe` v3.4.7.1).
- **Network Access:** Outbound HTTP/HTTPS internet connectivity to query search engines and download target documents.

### Step-by-Step Procedure

#### Step 1 — Create the Project
1. Launch **FOCA.exe** on the Windows machine.
2. In the top menu bar, click on **Project** $\rightarrow$ **New Project**.
3. In the project creation modal, populate the configuration fields:
   - **Project name:** Enter `FOCA-Lab`.
   - **Domain website:** Enter `owasp.org`.
   - **Alternative domains:** Leave blank (used only if secondary domains map to the same organizational scope).
   - **Project date:** Keep the current automated date.
   - **Project notes:** Enter `Ethical Hacking lab – passive OSINT and document metadata analysis`.
   - **Document download folder:** Click the browse button (`...`), navigate to `Documents`, create a new folder named `FOCA-Lab` (`Documents\FOCA-Lab`), and select it.
4. Click the **Create** button.
> *Note on Download Folder:* The document folder is the designated local staging directory where FOCA stores downloaded files and cached metadata tables.

#### Step 2 — Search for Public Documents
1. In the search engine selection panel on the left, check the boxes for **Google** and **Bing**.
2. *Search Engine Note:* **DuckDuckGo** may also be checked; however, due to frequent changes in third-party search APIs and CAPTCHA mechanisms, DuckDuckGo may return API error warnings.
3. Beside the **Extensions** heading, click **All** to automatically select all supported document formats (`.pdf`, `.doc`, `.docx`, `.xls`, `.xlsx`, `.ppt`, `.pptx`).
4. Click the **Search All** button in the lower section of the search pane.
5. Wait for the query routine to complete and the results table to populate with discovered file URLs.
> *Practical Observation Note:* Results vary based on search engine API updates and scraper compatibility. In our specific lab execution, the runtime log reported zero direct search scrape responses from Google and Bing and an operational error from DuckDuckGo; however, FOCA's parsing engine successfully populated the results table with indexed document URLs. Do not assume all configured search engines succeeded without checking the log output.

#### Step 3 — Download Documents
1. Click inside the document results table to focus the control.
2. Select the document rows to download (press `Ctrl+A` to highlight all selectable rows).
3. Right-click on the selected rows.
4. In the context menu, click **Download All** (or **Download** for individual items).
5. Monitor the download progress bar and status column.
6. Verify the download status indicators and verify file sizes in bytes.
> *Verification Warning:* A URL appearing in the results table indicates only that the search engine indexed a link; it does not guarantee the document is currently hosted, accessible, or successfully downloaded. Always check file sizes.

#### Step 4 — Extract Metadata
1. Select the downloaded documents in the table (rows displaying green download status icons).
2. Right-click the highlighted documents and select **Extract Metadata** (or click **Extract All Metadata** from the toolbar).
3. Wait for the extraction engine to parse the binary headers and XML structures of each file.
4. Review the **Document Analysis** pane and the **Metadata Summary** navigation tree:
   - **Folders:** Inspect internal file system paths, local disk structures (e.g., `C:\Users\...`, `/var/www/...`), and share names left by document creators.
   - **Software:** Identify operating system versions, word processing software (e.g., Microsoft Office versions, LibreOffice, LaTeX, Mac OS X Preview), and PDF generation libraries.
   - **Users:** Inspect usernames, authors, and email identifiers embedded in document properties.
   - **Printers:** Identify network printer names and queue paths discovered in print-job metadata.
   - **Files:** Inspect the total analyzed file list and individual extraction status.
> *Actual Lab Session Output:* The completed lab session showed **Files (10/10)** analyzed with the confirmation *"All documents were analyzed."* The metadata summary extracted **17 folders**, **8 software entries**, **0 users**, and **0 printers**. These represent the exact output from this run.  
> *Error Check Warning:* In our run, one document displayed a size of **0 B** and a **red X** indicator. A 0-byte file indicates a failed or interrupted download; it must be flagged as corrupt/empty and excluded from verified metadata deductions.

#### Step 5 — Save Evidence and Logs
1. Click **Save log to File** in the lower log panel and save the text log file inside the `Documents\FOCA-Lab` directory.
2. Capture screenshot evidence for your laboratory portfolio:
   - Search results table displaying indexed URLs.
   - Metadata Summary dashboard showing category counts.
   - Software results breakdown.
   - Folder results showing discovered internal paths.
   - Completed analysis window showing document status.

### What to Observe
- **Document Metadata:** Data about data stored within file headers (EXIF data, author properties, revision history, creation software, and internal filesystem paths).
- **Folders Category:** Reveals the author's internal folder structure, directory names, and operating system conventions (e.g., Windows drive letters vs. Unix directory separators).
- **Software Category:** Discloses internal client software stacks, word processors, and PDF converters, which an ethical hacker uses to identify unpatched client-side vulnerabilities.
- **Users Category:** Uncovers organizational usernames, login names, and email naming conventions, directly aiding social engineering or password-spraying assessments.
- **Printers Category:** Discloses internal network naming standards and printer models.
- **0 B File Warning:** A document showing 0 B indicates a failed HTTP download or broken link; its presence in the table does not represent analyzed content.

### Observation for the Lab Record
> **Observation (FOCA Practical Session):**  
> FOCA was used to search for publicly available documents associated with the selected domain (`owasp.org`). The documents were downloaded and analysed for metadata. The results included folder and software information. The metadata summary showed 17 folders and 8 software entries in this run. One file registered 0 B with a red indicator, demonstrating that failed downloads must be validated before reporting metadata. *(Note: Adjust folder and software counts if a fresh execution produces different values).*

### Result
Passive document reconnaissance and automated metadata harvesting for `owasp.org` were successfully executed using FOCA v3.4.7.1, uncovering internal operating directory structures and software version signatures without directly interacting with the target's web server.

### Viva Questions and Answers
1. **What is FOCA?**  
   *Answer:* FOCA (Fingerprinting Organizations with Collected Archives) is a Windows-based Open Source Intelligence tool that automates the discovery, downloading, and metadata analysis of public documents to uncover organizational intelligence.
2. **What is metadata?**  
   *Answer:* Metadata is structured "data about data" embedded within files, including author names, software versions, document creation dates, printer pathways, and internal file system paths.
3. **What is OSINT?**  
   *Answer:* OSINT stands for Open Source Intelligence. It is the practice of legally collecting and analyzing publicly accessible data (such as search-engine results, DNS tables, and published documents) to evaluate an organization's security posture.
4. **What information can FOCA extract from documents?**  
   *Answer:* FOCA extracts usernames, operating systems, software vendors and version numbers, internal file server folder paths, email addresses, and network printer names.
5. **Why must download and extraction errors be checked?**  
   *Answer:* A search engine may index a URL that is currently deleted, access-restricted, or corrupted. If a file downloads as 0 bytes (showing a red error mark), it contains no valid document data and must not be reported as a successful metadata source.

### Quick Revision
- **FOCA:** Automated document metadata extractor developed by ElevenPaths.
- **Prerequisite:** Microsoft SQL Server Express / LocalDB.
- **File Formats:** PDF, DOC, DOCX, XLS, XLSX, PPT, PPTX.
- **Core Tabs:** Folders, Software, Users, Printers, Network.
- **Search Logic:** Generates targeted search engine queries (`site:domain filetype:ext`).
- **Data Integrity:** URLs do not guarantee valid downloads; always check file size and extraction status.
- **Remediation:** Organizations must sanitize or strip metadata before publishing public documents.

---

## 2. Tool 2: Maltego (Visual Link Analysis & OSINT Infrastructure Mapping)

### Aim
To construct a visual Open Source Intelligence (OSINT) relationship graph for the target domain `ssn.edu.in` using Maltego Community Edition, mapping its subdomains, email infrastructure, and network IP addresses through automated transforms.

### Introduction
Maltego is an interactive visual link-analysis platform designed for cyber intelligence, digital forensics, and OSINT reconnaissance. Rather than presenting intelligence as flat text lists, Maltego models information as discrete "entities" (such as domains, IP addresses, email addresses, and organizations) interconnected by directional relationships on a visual canvas. Analysts execute queries called "transforms" that query public records and third-party APIs to uncover linked assets.

- **Platform:** Maltego Community Edition (CE) on Windows / Kali Linux.
- **Target Domain:** `ssn.edu.in`.
- **Mode:** Passive OSINT relationship mapping.

### Requirements
- **Operating System:** Windows 10 / 11, Linux, or macOS.
- **Java Runtime:** Java Development Kit (JDK) or Java Runtime Environment (JRE) 11 or 17 (x64).
- **Account:** Registered Maltego Community Edition account (free registration via `maltego.com`).
- **Software Package:** Maltego Desktop client (Community Edition).
- **Network Access:** Unrestricted outbound HTTPS access to Maltego transform servers and public data providers.

### Step-by-Step Procedure

#### Step 1 — Create a Graph
1. Launch the **Maltego** application on Windows.
2. If prompted, sign in with your verified Maltego Community Edition account credentials.
3. In the upper-left corner of the application window, click **File** $\rightarrow$ **New Graph** (or click the **New Graph** icon on the quick-access toolbar).
4. Wait for the blank visual graph canvas to initialize with its grid background.

#### Step 2 — Add a Domain Entity
1. Locate the **Entity Palette** panel docked on the left side of the window.
2. In the Entity Palette search bar, type `Domain` (or expand the **Infrastructure** category).
3. Left-click and drag the **Domain** entity icon onto the center of the blank canvas.
4. Double-click the default entity text (`paterva.com`).
5. Type the target domain: `ssn.edu.in`.
6. Press **Enter** or click outside the text box to confirm the target name.
> *Concept Note:* An **Entity** is a standardized data object representing an asset—such as a Domain, DNS Name, Email Address, Person, or IP Address—possessing unique properties and visual icons.

#### Step 3 — Discover DNS Names
1. Right-click on the `ssn.edu.in` Domain entity node on the canvas to open the transform context menu.
2. In the transform search bar at the top of the menu, type `dns`.
3. Locate the DNS name discovery transforms. In our verified lab workflow, select:
   ```text
   [Utilities] To DNS Name (interesting) [SecurityTrails]
   ```
4. Click the **Run** (play) button beside the transform.
5. Wait for the transform progress bar to complete.
6. Inspect the newly populated **DNS Name** entities (blue/orange sphere icons) linked by directional arrows to the parent domain.
> *Operational Note:* Transform availability and returned entity counts depend on Maltego CE API key quotas, installed transform hubs, and external service availability. If the SecurityTrails transform is unavailable, select the standard built-in DNS transform `To DNS Name - [DNS]` or `To DNS Name - hosts (Server IP)`.

#### Step 4 — Discover Email Addresses
1. Click on the central `ssn.edu.in` Domain entity again to select it.
2. Right-click to open the transform context menu.
3. In the transform search bar, type `email`.
4. Select the relevant email discovery transform (in our workflow, choose the second available option, such as `To Email Address [SEC/PGP]` or `To Email Address [Using Search Engine]`).
5. Click **Run** and wait for the transform to execute.
6. Inspect the canvas for **Email Address** entities, represented by `@` envelope icons.
> *OSINT Integrity Rule:* Public email transforms may return zero results depending on domain privacy and search engine rate limits. Never fabricate email addresses or placeholder names in the graph.

#### Step 5 — Find IP Infrastructure
1. Identify a discovered **DNS Name** entity on the canvas (an orange/blue node representing a host, e.g., `mail.somca.ssn.edu.in` or an equivalent subdomain returned in your run).
2. Right-click specifically on that **DNS Name** entity (do not right-click an email or domain entity).
3. In the transform search bar, type `IP`.
4. Select the transform `To IP Address [DNS]` (or the available DNS-to-IP resolution transform).
5. Click **Run** and await resolution.
6. Inspect the resulting **IPv4 Address** entities linked directly to the selected DNS hostname.
> *Fallback Guidance:* If `mail.somca.ssn.edu.in` is not returned in your specific graph, choose any valid subdomain entity present on your canvas (such as `www.ssn.edu.in` or `pdns.ssn.edu.in`) to perform the IP resolution.

#### Step 6 — Analyze the Completed Graph
1. Review the full topology of the generated graph, which models:
   $$\text{Domain (ssn.edu.in)} \longrightarrow \text{Discovered Subdomains (DNS Names)}$$
   $$\text{Domain (ssn.edu.in)} \longrightarrow \text{Discovered Email Addresses (@)}$$
   $$\text{DNS Names} \longrightarrow \text{Host IPv4 Addresses}$$
2. Understand the core Maltego components:
   - **Entity:** A discrete information node representing an asset.
   - **Transform:** A specific query that takes an input entity and queries OSINT databases to discover linked child entities.
   - **Graph:** The dynamic visual network showing multi-tier relationships between discovered nodes.
3. Reorganize the layout using the **Layouts** toolbar (e.g., Organic Layout, Hierarchical Layout, or Circular Layout) to improve readability.

### What to Observe
- **Entity Node Representation:** Distinct icons represent different asset categories (e.g., globe icon for Domain, gear/sphere icon for DNS Name, envelope for Email, computer/rack for IP Address).
- **Directional Links:** Directed arrows depict relationships discovered by transforms (e.g., Domain $\rightarrow$ Subdomain indicates DNS delegation/linkage).
- **Shared Infrastructure:** If multiple DNS entities connect to a single IP address entity, this reveals shared web hosting or server consolidation.
- **External Data Dependencies:** Community Edition transforms route through public servers; rate limiting or stale public DNS records may affect real-time accuracy.

### Observation for the Lab Record
> **Observation (Maltego Practical Session):**  
> Maltego was used to create a Domain entity for `ssn.edu.in`. DNS and email transforms were executed to discover related entities. An IP-address transform was then run on a DNS-name entity. The resulting graph displayed the relationships between the domain, the discovered DNS names, available email addresses and related IP addresses. *(Note: Adjust this observation if a particular transform returned zero results in your session).*

### Result
Visual OSINT link analysis and infrastructure mapping for `ssn.edu.in` were successfully achieved using Maltego Community Edition, mapping the hierarchical relationships between domain assets, subdomains, and host IP addresses on an interactive canvas.

### Viva Questions and Answers
1. **What is Maltego?**  
   *Answer:* Maltego is an open-source intelligence and visual link-analysis tool that queries public databases to map relationships between digital assets, people, and network infrastructure as an interactive graph.
2. **What is OSINT?**  
   *Answer:* Open Source Intelligence (OSINT) is the legal collection and correlation of information from publicly accessible sources (search engines, DNS registries, WHOIS databases) to build an intelligence profile of a target.
3. **What is an entity in Maltego?**  
   *Answer:* An entity is an individual piece of data represented as a distinct node on the Maltego canvas, such as a Domain, DNS Name, IP Address, Email Address, or Person.
4. **What is a transform?**  
   *Answer:* A transform is a focused, automated query or script that takes an input entity, executes lookups against external databases or APIs, and returns related output entities onto the graph.
5. **What is the purpose of a graph in Maltego?**  
   *Answer:* The graph visually depicts the interconnections and attack surface between disparate entities, allowing security analysts to spot hidden relationships, shared infrastructure, and single points of failure that flat text lists cannot reveal.

### Quick Revision
- **Maltego:** Visual link-analysis and graph-based OSINT platform.
- **Entity:** Standardized node (Domain, IP, DNS Name, Email).
- **Transform:** Query script discovering related entities from public APIs.
- **Graph:** Interactive topological map of entities and links.
- **Target Domain:** `ssn.edu.in`.
- **Layout Views:** Hierarchical, Organic, Circular.
- **Scope Restriction:** Focuses strictly on passive relationship discovery; no unauthorized access or exploitation.

---

## 3. Tool 3: Sysmon (System Monitor & Endpoint Activity Logging)

### Aim
To configure Microsoft Sysmon (System Monitor) on a Windows system using the SwiftOnSecurity configuration template and monitor detailed endpoint security events via the Windows Event Viewer.

### Introduction
Sysmon (System Monitor) is a Microsoft Sysinternals utility that runs as a background Windows service and device driver to monitor and log security-critical system activity to the Windows Event Log. Unlike standard Windows audit policies, Sysmon provides granular tracking of process execution, command-line arguments, network sockets, file modifications, driver loads, and DNS queries. It serves as an essential endpoint detection and response (EDR) data source for threat hunting and blue-team analysis.

- **Developer:** Microsoft Sysinternals (Mark Russinovich).
- **Configuration Reference:** SwiftOnSecurity Sysmon Config ([https://github.com/SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)).
- **Event Log Channel:** `Microsoft-Windows-Sysmon/Operational`.

### Requirements
- **Operating System:** Windows 10, Windows 11, or Windows Server (x64).
- **Administrative Privileges:** Elevated local administrator permissions (required to install services and modify kernel drivers).
- **Shell Environment:** Windows PowerShell (Run as Administrator) or Elevated Command Prompt.
- **Sysmon Binary:** `Sysmon64.exe` (from the Microsoft Sysinternals Suite).
- **Version Control Tool:** Git for Windows (or manual browser download of configuration files).

### Step-by-Step Procedure

#### Step 1 — Prepare the Configuration Folder
1. Click the **Start** menu, search for **PowerShell**, right-click **Windows PowerShell**, and select **Run as Administrator**.
2. Create a dedicated working directory `C:\sys` and set it as your working location:
   ```powershell
   New-Item -ItemType Directory -Path C:\sys -Force
   Set-Location C:\sys
   ```
   > *Folder Handling Rule:* If the `C:\sys` directory already exists, the `-Force` switch safely ensures no error occurs, and `Set-Location C:\sys` navigates into the existing directory without creating redundant nested folders.

#### Step 2 — Download the SwiftOnSecurity Configuration
1. In the elevated PowerShell prompt, clone the industry-standard SwiftOnSecurity Sysmon configuration repository:
   ```powershell
   git clone https://github.com/SwiftOnSecurity/sysmon-config.git
   ```
2. The Git command downloads the repository files into `C:\sys\sysmon-config`.
3. Verify that the configuration file exists:
   ```powershell
   Get-ChildItem C:\sys\sysmon-config
   ```
   Ensure `sysmonconfig-export.xml` is present in the directory listing.
   > *Handling Existing Repositories:* If Git reports that destination path `C:\sys\sysmon-config` already exists, do not delete it. Simply verify that `sysmonconfig-export.xml` is inside it, or update it with `git pull`.

#### Step 3 — Apply the Configuration
1. Execute the configuration update command:
   ```powershell
   Sysmon64.exe -c .\sysmon-config\sysmonconfig-export.xml
   ```
2. Verify that the command prompt outputs confirmation:
   ```text
   Configuration file validated.
   Configuration updated.
   ```
3. **Understanding the `-c` Flag:** The `-c` command-line switch updates the configuration rules of an *already installed and running* Sysmon service without requiring a service reboot or re-installation.
4. **Path Resolution & Absolute Paths:** If `Sysmon64.exe` is not located in the current working directory (`C:\sys`) or registered in your system `PATH`, provide the absolute path to the executable (e.g., if installed in `C:\Sysmon`):
   ```powershell
   C:\Sysmon\Sysmon64.exe -c C:\sys\sysmon-config\sysmonconfig-export.xml
   ```
   > *Critical Distinction (Installation vs. Configuration Update):* The command `Sysmon64.exe -c` does **not** install Sysmon; it only updates rules. If Sysmon is not yet installed on the machine, run the initial installation command first:
   > ```powershell
   > Sysmon64.exe -accepteula -i C:\sys\sysmon-config\sysmonconfig-export.xml
   > ```

#### Step 4 — Open Event Viewer
1. Press the keyboard shortcut `Win + R` to open the Windows Run dialog.
2. Type `eventvwr.msc` and press **Enter** (or click OK).
3. In the left navigation tree of **Event Viewer**, expand the following path:
   ```text
   Event Viewer (Local)
   └── Applications and Services Logs
       └── Microsoft
           └── Windows
               └── Sysmon
                   └── Operational
   ```
4. Click directly on **Operational** to view the active Sysmon log entries.

#### Step 5 — Inspect System Events
1. In the center pane, observe the stream of logged system events.
2. Select any individual event in the upper list to display its metadata in the lower pane.
3. Click the **General** tab to read human-readable descriptions of the activity (e.g., process image name, command line, process ID, user account, parent process, hashes).
4. Click the **Details** tab and choose **XML View** or **Friendly View** to inspect structured parameters.
5. Identify common security-relevant Sysmon Event IDs:
   - **Event ID 1:** Process Creation (identifies malware execution, command-line arguments, parent-child process spoofing).
   - **Event ID 3:** Network Connection (records outbound/inbound TCP/UDP sockets, remote IPs, ports, and initiating process).
   - **Event ID 11:** File Create (detects executable drops, ransomware staging, or payload delivery).
   - **Event ID 13:** Registry Value Set (tracks persistence mechanisms in autorun registry keys).
   - **Event ID 22:** DNS Query (logs domain names requested by processes, detecting C2 beacons).

### What to Observe
- **Event ID & Task Category:** Quantifies the exact behavior category (Process Creation, Network Connect, DNS Query).
- **Command Line Field (Event ID 1):** Captures full obfuscated arguments passed to interpreters like `powershell.exe`, `cmd.exe`, or `mshta.exe`.
- **Hashes Field:** Reports MD5, SHA256, and IMPHASH of loaded binaries, enabling automated correlation with threat intelligence platforms (VirusTotal).
- **ParentCommandLine & ParentImage:** Exposes process execution ancestry (e.g., detecting `winword.exe` spawning `powershell.exe`).
- **User Context:** Discloses the security token under which an action occurred (e.g., `NT AUTHORITY\SYSTEM` vs. standard user).

### Observation for the Lab Record
> **Observation (Sysmon Configuration & Logging):**  
> Sysmon was configured using the SwiftOnSecurity configuration file. The configuration was validated and updated successfully. Windows Event Viewer was opened to inspect events under Sysmon → Operational. The available events were reviewed to observe recorded system activity, verifying process creation (Event ID 1) and system events.

### Result
Microsoft Sysmon was successfully configured and updated with customized XML telemetry rules, and endpoint monitoring was verified through real-time event log inspection in Windows Event Viewer.

### Viva Questions and Answers
1. **What is Sysmon?**  
   *Answer:* Sysmon (System Monitor) is a Microsoft Sysinternals tool that runs as a Windows background service and kernel driver to record detailed security events (process creation, network sockets, file changes) in the Windows Event Log.
2. **What is the purpose of the configuration file?**  
   *Answer:* The XML configuration file defines filtering rules (include/exclude logic) to filter out benign system noise and focus logging on high-value threat indicators (suspicious command lines, unquoted paths, C2 network connections).
3. **What does the `-c` option do in Sysmon?**  
   *Answer:* The `-c` option updates or verifies the configuration rules of an already installed and running Sysmon instance without requiring service re-installation.
4. **Where are Sysmon events stored?**  
   *Answer:* Sysmon events are stored in the Windows Event Log under:  
   `Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`.
5. **What is the difference between Sysmon and Event Viewer?**  
   *Answer:* Sysmon is an endpoint monitoring agent/driver that *generates* detailed security event telemetry; Event Viewer is the administrative graphical console used to *view, filter, and inspect* those generated logs.

### Quick Revision
- **Sysmon:** Microsoft Sysinternals endpoint telemetry service and kernel driver.
- **Log Path:** `Applications and Services Logs \ Microsoft \ Windows \ Sysmon \ Operational`.
- **Configuration Update:** `Sysmon64.exe -c <config.xml>`.
- **Initial Install:** `Sysmon64.exe -accepteula -i <config.xml>`.
- **Uninstall:** `Sysmon64.exe -u force`.
- **Key Event IDs:**
  - **ID 1:** Process Creation (Executable path, parent process, command line, hashes).
  - **ID 3:** Network Connection (Local/remote IP, port, protocol, process).
  - **ID 11:** File Create.
  - **ID 13:** Registry Event.
  - **ID 22:** DNS Query.
- **Config Rule:** SwiftOnSecurity template balances comprehensive visibility with low log noise.

---

## 4. Combined Revision Section

### Overall Tool Comparison Matrix

| Tool | Category | Main Task | Main Output |
| :--- | :--- | :--- | :--- |
| **FOCA** | Windows OSINT / Document Reconnaissance | Find publicly available documents via search engines and extract embedded metadata. | Document metadata summary, internal folder pathways, software version signatures, usernames, and printer records. |
| **Maltego** | Visual OSINT Link Analysis | Build a graphical relationship topology using automated transforms across public entities. | Connected interactive graph mapping domains, subdomains (DNS names), email addresses, and server IP infrastructure. |
| **Sysmon** | Windows Endpoint Monitoring & EDR | Monitor low-level Windows activity via custom XML configuration rules. | High-fidelity security activity records (Event IDs 1, 3, 11, 13, 22) logged in Windows Event Viewer. |

### High-Level Functional Distinction
- **FOCA** examines *document metadata* to reveal internal organizational footprints.
- **Maltego** maps *relationships between OSINT entities* to visualize external attack surfaces.
- **Sysmon** monitors *activity on a Windows system* to detect endpoint execution and anomalous behavior.

### One-Minute Record Summary (Aim, Procedure, Observation, Result)

#### 1. FOCA
- **Aim:** To extract hidden metadata from publicly available documents of a target domain (`owasp.org`) using FOCA.
- **Procedure:** Open FOCA 3.4.7.1, create project `FOCA-Lab` with download folder `Documents/FOCA-Lab`, search documents using Google and Bing, download discovered files, extract metadata, and inspect Folders, Software, Users, and Printers.
- **Observation:** FOCA searched and analyzed public documents for `owasp.org`; metadata summary revealed 17 folders and 8 software entries in this run, while a 0 B file with a red X highlighted the need to verify downloads.
- **Result:** Document metadata extraction was successfully executed, revealing internal filesystem paths and software versions.

#### 2. Maltego
- **Aim:** To map visual OSINT relationships for target domain `ssn.edu.in` using Maltego Community Edition.
- **Procedure:** Open Maltego, create a New Graph, add a Domain entity for `ssn.edu.in`, run DNS transforms to discover subdomains, run email transforms to locate addresses, and run IP transforms on a DNS entity.
- **Observation:** Maltego mapped `ssn.edu.in` to its DNS names, available email entities, and host IP addresses, visually depicting infrastructure interconnections.
- **Result:** Visual link analysis successfully mapped the target domain's digital assets into an interactive graph.

#### 3. Sysmon
- **Aim:** To configure Microsoft Sysmon with the SwiftOnSecurity template and inspect system activity events in Event Viewer.
- **Procedure:** Create `C:\sys`, clone `sysmon-config`, update configuration using `Sysmon64.exe -c .\sysmon-config\sysmonconfig-export.xml`, open `eventvwr.msc`, navigate to `Microsoft-Windows-Sysmon/Operational`, and inspect event properties.
- **Observation:** Sysmon configuration was validated and updated successfully; Event Viewer recorded endpoint events including Process Creation (Event ID 1).
- **Result:** Windows endpoint telemetry logging was successfully configured and verified using Event Viewer.
