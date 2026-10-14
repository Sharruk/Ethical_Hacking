# Ethical Hacking Laboratory: Other GUI Tools Practical Notes
**Course:** ICS1561 – Ethical Hacking Laboratory  
**Category:** 02 – Windows and GUI Tools / 02 – Other GUI Tools  
**Target Environment:** Kali Linux (VirtualBox) / Isolated Lab Network  
**Tools Covered:** Wireshark (Network Packet Analysis) & Armitage (GUI Front End for Metasploit)  
**Reference Manuals:** Week 5 (Wireshark Traffic Analysis), Week 9 (Armitage Lab Manual)

---

## Table of Contents
1. [Tool 1: Wireshark (Network Packet Analysis)](#1-tool-1-wireshark-network-packet-analysis)
   - [Aim](#aim)
   - [Introduction](#introduction)
   - [Step-by-Step Procedure](#step-by-step-procedure)
   - [Display Filters Reference](#display-filters-reference)
   - [What to Observe](#what-to-observe)
   - [Observation for the Lab Record](#observation-for-the-lab-record)
   - [Result](#result)
   - [Viva Questions and Answers](#viva-questions-and-answers)
   - [Quick Revision](#quick-revision)
2. [Tool 2: Armitage (GUI Front End for Metasploit)](#2-tool-2-armitage-gui-front-end-for-metasploit)
   - [Aim](#aim-1)
   - [Introduction](#introduction-1)
   - [Step-by-Step Procedure](#step-by-step-procedure-1)
   - [Interface Layout and Visual Observations](#interface-layout-and-visual-observations)
   - [Exploit Ranking vs. Vulnerability Severity](#exploit-ranking-vs-vulnerability-severity)
   - [What to Observe](#what-to-observe-1)
   - [Observation for the Lab Record](#observation-for-the-lab-record-1)
   - [Result](#result-1)
   - [Viva Questions and Answers](#viva-questions-and-answers-1)
   - [Quick Revision](#quick-revision-1)
3. [Comparison and Comprehensive Practical Revision](#3-comparison-and-comprehensive-practical-revision)
   - [Overall Tool Comparison](#overall-tool-comparison)
   - [One-Minute Record Summary (Aim, Procedure, Observation, Result)](#one-minute-record-summary-aim-procedure-observation-result)
   - [Final Practical Exam Viva Checklist](#final-practical-exam-viva-checklist)

---

## 1. Tool 1: Wireshark (Network Packet Analysis)

### Aim
To capture, decode, and analyze live network packets on an active network interface using Wireshark, inspect multi-layer protocol headers of generated ICMP traffic, apply display filters, and manage capture sessions.

### Introduction
Wireshark is an industry-standard, graphical network protocol analyzer used to capture, decode, and inspect packet transmissions passing through a system's network interface card (NIC). Operating in promiscuous or standard capture mode, Wireshark dissects traffic across all OSI model layers—from physical frames and data link Ethernet headers up to application payloads (HTTP, DNS). It enables security analysts to examine live communications, troubleshoot protocol faults, and detect unencrypted sensitive data transfers.

> *Environment Note:* Wireshark comes pre-installed in standard Kali Linux virtual machines. If installation or reinstallation is needed, it can be installed via:
> ```bash
> sudo apt update && sudo apt install -y wireshark
> ```

---

### Step-by-Step Procedure

#### Step 1 — Open Wireshark and Select Interface
1. Start the **Kali Linux VM** inside Oracle VirtualBox.
2. Open **Wireshark** from the Kali desktop application menu (under *09 - Sniffing & Spoofing* $\rightarrow$ *Wireshark*, or by opening a terminal and running `wireshark &`).
3. In the Wireshark welcome screen, locate the list of available network capture interfaces.
4. Identify the active interface displaying dynamic packet activity sparklines (in our verified virtual lab environment, interface `eth0` is used).
5. Double-click the active interface (e.g., `eth0`) to begin live packet capture immediately.
6. Leave the capture running in the background.
> *Interface Selection Note:* The designated capture interface depends on your virtual machine's network configuration (e.g., `eth0` for Host-Only or NAT interfaces, `wlan0` for wireless adapters, or `any` to capture across all interfaces simultaneously).

#### Step 2 — Generate Test Traffic
1. Keep the Wireshark capture running in the background.
2. Open a new Kali Linux terminal window by pressing `Ctrl + Alt + T`.
3. Generate controlled network traffic using the `ping` utility by running:
   ```bash
   ping 1.1.1.1
   ```
4. Observe several echo replies (or timeout messages) printed to the terminal.
5. Press `Ctrl + C` in the terminal to terminate the ping operation.
6. Return to the active Wireshark window.
7. Click the red square **Stop** button located in the upper-left toolbar to halt the packet capture.

> *Traffic Analysis Note:* The `ping` command generates Internet Control Message Protocol (ICMP) traffic—specifically ICMP Echo Requests (Type 8) and Echo Replies (Type 0). If the external target `1.1.1.1` does not respond due to lab internet restrictions, ping your local default gateway (e.g., `ping 10.0.2.2`) or another authorized lab VM (e.g., Metasploitable). A lack of internet replies does not mean the packet capture failed; Wireshark still captures outbound Echo Requests.

#### Step 3 — Apply Display Filters
1. Locate the green **Display Filter** bar at the top of the Wireshark interface (below the main toolbar).
2. Type a filter syntax and press **Enter** (or click the blue right-arrow apply button).
3. Observe how Wireshark filters the displayed packets in the Packet List pane.
4. Practice applying the core display filters summarized below:

| Filter | Purpose | Example Observation |
| :--- | :--- | :--- |
| `icmp` | Displays only ICMP packets | Isolates generated ping Echo Requests (Type 8) and Echo Replies (Type 0). |
| `tcp` | Displays all Transmission Control Protocol packets | Shows three-way handshakes (SYN, SYN-ACK, ACK) and active TCP sessions. |
| `udp` | Displays all User Datagram Protocol packets | Reveals connectionless datagram traffic, including DNS queries and DHCP. |
| `dns` | Displays Domain Name System queries and responses | Isolates hostname resolution requests (A, AAAA, PTR records). |
| `http` | Displays unencrypted Hypertext Transfer Protocol traffic | Exposes plaintext GET/POST requests, response codes (200 OK, 302 Found), and headers. |

> *Crucial Distinction:* A **display filter** changes only the packets currently rendered visible in the list pane. It does **not** discard or delete unshown packets from the active capture buffer. Clearing the filter (`Ctrl + Backspace` or clicking the clear `X` icon) instantly restores all captured packets.

#### Step 4 — Inspect Packet Protocol Layers
1. Clear any active display filter by clicking the **Clear** (`X`) button in the filter bar.
2. Left-click an individual packet in the top **Packet List** pane (e.g., an ICMP packet or HTTP request).
3. Look at the middle **Packet Details** pane, which displays the decoded protocol layer tree:
   - Expand **Frame:** Displays the packet index number, exact timestamp, arrival time, and total frame length on the wire (e.g., 98 bytes).
   - Expand **Ethernet II:** Displays the Data Link layer source and destination hardware Media Access Control (MAC) addresses (e.g., `08:00:27:...` for VirtualBox NICs).
   - Expand **Internet Protocol Version 4 (IPv4):** Displays the Network layer Source IP address, Destination IP address, Time to Live (TTL), Header Length, and Protocol field (e.g., `Protocol: ICMP (1)` or `Protocol: TCP (6)`).
   - Expand **Transmission Control Protocol (TCP)** or **User Datagram Protocol (UDP):** If the selected packet is a transport-layer packet, inspect Source Port, Destination Port, Sequence/Acknowledgment numbers, and TCP control flags (SYN, ACK, FIN, RST).
   - Expand **Internet Control Message Protocol (ICMP):** If an ICMP packet is selected, inspect Type (Type 8 for Request, Type 0 for Reply), Code (0), Checksum, Identifier, and Sequence Number.
4. Look at the bottom **Packet Bytes** pane, which shows the raw hexadecimal representation of the packet alongside its ASCII printable text equivalent.

> *Important Technical Distinction:* ICMP ping packets operate directly above the Network layer (IPv4 protocol number 1) and **do not contain TCP or UDP port numbers**. Source and destination port fields exist only within transport-layer protocols such as TCP (protocol 6) and UDP (protocol 17).

#### Step 5 — Save the Packet Capture
1. Ensure the live capture is fully stopped (red square icon is inactive).
2. Click **File** $\rightarrow$ **Save As...** from the top menu bar.
3. In the file save dialog, navigate to the Kali user's home directory (`/home/kali/`).
4. Enter the filename:
   ```text
   wireshark-practice.pcapng
   ```
5. Leave the default file format as **Wireshark/tcpdump/... - pcapng (*.pcapng)**.
6. Click **Save**.
7. Expected full path: `/home/kali/wireshark-practice.pcapng`.

#### Step 6 — Open and Verify the Saved Capture
1. In Wireshark, click **File** $\rightarrow$ **Close** to clear the current workspace.
2. Click **File** $\rightarrow$ **Open...** (or press `Ctrl + O`).
3. Navigate to `/home/kali/` and select `wireshark-practice.pcapng`.
4. Click **Open**.
5. Verify that all previously captured packets and protocol trees reload accurately.

---

### Display Filters Reference

```text
========================================================================================
WIRESHARK DISPLAY FILTER CHEAT SHEET
========================================================================================
Protocol Isolation:
  icmp                                -> Show all ICMP ping / error packets
  tcp                                 -> Show all TCP traffic
  udp                                 -> Show all UDP traffic
  dns                                 -> Show all DNS queries and answers
  http                                -> Show unencrypted HTTP traffic
  arp                                 -> Show Address Resolution Protocol requests/replies

IP Address Filtering:
  ip.addr == 10.0.2.2                 -> Traffic involving IP 10.0.2.2 (source or dest)
  ip.src == 10.0.2.15                 -> Packets originating from 10.0.2.15
  ip.dst == 1.1.1.1                   -> Packets sent to destination 1.1.1.1

Port Filtering:
  tcp.port == 80                      -> HTTP traffic on port 80
  tcp.port == 443                     -> HTTPS traffic on port 443
  udp.port == 53                      -> DNS queries on UDP port 53

Logical Operators:
  icmp and ip.src == 10.0.2.15        -> Match both conditions
  tcp.port == 80 or tcp.port == 8080  -> Match either web port
  !arp (or not arp)                   -> Exclude ARP broadcasts from view
========================================================================================
```

---

### What to Observe
- **Packet List Pane (Top):** Shows packet number, timestamp (relative to capture start), source IP, destination IP, protocol name, length in bytes, and high-level summary info.
- **Packet Details Pane (Middle):** Collapsible hierarchical tree representing the OSI layers (Frame $\rightarrow$ Ethernet II $\rightarrow$ IPv4 $\rightarrow$ Protocol).
- **Packet Bytes Pane (Bottom):** Raw data dump displaying 16 hex bytes per row with ASCII text translation on the right.
- **ICMP Characteristics:** Ping packets contain Type and Code fields rather than port numbers.
- **MAC vs. IP Addresses:** Ethernet II exposes local physical hardware addresses (hop-by-hop), while IPv4 reveals end-to-end logical addresses.
- **Data Security:** Plaintext protocols (HTTP, FTP, Telnet) display sensitive parameters and text directly in the bytes pane, proving why encrypted protocols (HTTPS, SSH) are essential.

---

### Observation for the Lab Record
> **Observation (Wireshark Network Capture):**  
> Wireshark was used to capture network traffic on the selected interface (`eth0`). ICMP traffic was generated using the `ping` command and captured. Packets were inspected to identify protocol details, source and destination addresses, and other available fields. Display filters (`icmp`, `tcp`, `udp`) were applied to isolate specific protocols without altering the underlying capture. The capture was saved as `/home/kali/wireshark-practice.pcapng` and reopened for verification.

---

### Result
Network traffic on the virtual interface was successfully captured, filtered, and analyzed using Wireshark, identifying multi-layer protocol headers and verifying capture persistence via the `.pcapng` file format.

---

### Viva Questions and Answers

1. **What is Wireshark?**  
   *Answer:* Wireshark is a free, open-source graphical packet analyzer used for network troubleshooting, security analysis, protocol development, and packet-level inspection across all standard networking layers.
2. **What is a packet capture?**  
   *Answer:* A packet capture is the interception and recording of raw data packets traversing a computer network interface over a period of time, stored for forensic or diagnostic examination.
3. **What is a display filter?**  
   *Answer:* A display filter is an expression applied to an existing packet capture to temporarily show only packets meeting specific criteria (e.g., `icmp`, `ip.addr == 10.0.2.2`) without deleting non-matching packets from memory.
4. **What is the difference between a MAC address and an IP address?**  
   *Answer:* A MAC address is a 48-bit physical hardware identifier burned into the network interface card operating at Layer 2 (Data Link) for local segment delivery. An IP address is a 32-bit (IPv4) or 128-bit (IPv6) logical address operating at Layer 3 (Network) used for global end-to-end routing.
5. **What is the difference between TCP and UDP?**  
   *Answer:* TCP (Transmission Control Protocol) is a connection-oriented, reliable protocol that uses a three-way handshake, sequencing, and acknowledgments to guarantee delivery. UDP (User Datagram Protocol) is a connectionless, lightweight protocol that transmits datagrams without establishing connections or verifying packet arrival.
6. **What is the `.pcapng` format?**  
   *Answer:* PCAP Next Generation (`.pcapng`) is an advanced packet capture storage standard that improves upon legacy `.pcap` by supporting captures from multiple interfaces simultaneously, nanosecond timestamps, packet comments, and interface metadata.
7. **What is the difference between a capture filter and a display filter?**  
   *Answer:* A capture filter uses Berkeley Packet Filter (BPF) syntax (e.g., `host 10.0.2.2`) and is evaluated *before* capturing; non-matching packets are discarded immediately. A display filter uses Wireshark syntax (e.g., `ip.addr == 10.0.2.2`) and is evaluated *after* capturing, hiding non-matching packets from view while preserving the complete capture.

---

### Quick Revision
- **Wireshark:** Network packet sniffer and protocol analyzer.
- **Active Interface:** `eth0` (virtual machine Ethernet adapter).
- **Test Command:** `ping 1.1.1.1` (generates ICMP Echo Request/Reply).
- **Three Panes:** Packet List (top), Packet Details (middle), Packet Bytes (bottom).
- **Layer Mapping:** Frame (Physical) $\rightarrow$ Ethernet II (Data Link MAC) $\rightarrow$ IPv4 (Network IP) $\rightarrow$ Transport (TCP/UDP) or ICMP.
- **ICMP Feature:** Has Type/Code, **no port numbers**.
- **Display Filters:** `icmp`, `tcp`, `udp`, `dns`, `http`, `ip.addr == <IP>`, `tcp.port == <Port>`.
- **File Format:** `.pcapng` saved in `/home/kali/wireshark-practice.pcapng`.

---

## 2. Tool 2: Armitage (GUI Front End for Metasploit)

### Aim
To perform graphical host discovery, service enumeration, and attack-module analysis on an authorized virtual lab network using Armitage, the GUI front end for the Metasploit Framework.

### Introduction
Armitage is a graphical cyber attack management tool developed for the Metasploit Framework. It visualizes targets on an interactive canvas, organizes host assets by operating system, recommends applicable exploit modules, and manages multi-target penetration testing sessions. Armitage connects directly to the Metasploit RPC daemon (`msfrpcd`) backed by a PostgreSQL database, streamlining reconnaissance and vulnerability correlation into an intuitive point-and-click interface.

- **Developer:** Raphael Mudge.
- **Backend Infrastructure:** Metasploit Framework & PostgreSQL database (`msfdb`).
- **Platform:** Kali Linux (x64) in VirtualBox.
- **Operational Rule:** Used strictly on isolated, instructor-authorized lab networks.

---

### Step-by-Step Procedure

#### Step 1 — Check the Metasploit Database
If Armitage is already open and connected, skip this setup step. Otherwise, verify database readiness:
1. Open a Kali terminal (`Ctrl + Alt + T`).
2. Check the current PostgreSQL database service status:
   ```bash
   sudo msfdb status
   ```
3. How to proceed based on the output:
   - **If the database is running:** Proceed directly to launch Armitage.
   - **If the database is stopped:** Start the service by running:
     ```bash
     sudo msfdb start
     ```
   - **Only if the database has never been initialized:** Initialize it once using:
     ```bash
     sudo msfdb init
     ```
4. Launch Armitage with administrative privileges:
   ```bash
   sudo armitage
   ```
5. In the connection prompt modal (`127.0.0.1:55553`), click **Connect**. If prompted to start the Metasploit RPC server (`msfrpcd`), click **Yes** and allow Armitage to initialize.

> *Database Role Note:* The Metasploit PostgreSQL database stores discovered host IP addresses, open port numbers, service banners, vulnerability leads, and open shells. It only needs to be initialized once per machine; do not re-run `msfdb init` on every startup.

#### Step 2 — Understand the User Interface
Once launched, inspect the four primary functional panels of Armitage:
1. **Modules Panel (Upper-Left):** Collapsible hierarchical tree displaying Metasploit module categories: `auxiliary`, `exploit`, `payload`, and `post`. Includes an instant keyword search bar.
2. **Main Targets Canvas (Upper-Right):** Visual workspace displaying discovered hosts as computer icons, annotated with hostnames, IP addresses, operating system icons, and compromise status.
3. **Console Panel (Bottom):** Multi-tabbed terminal area displaying scan output (`Console`, `nmap` tabs) and direct Metasploit command-line access (`msf >`).
4. **Session Management (Dynamic Tabs):** When an active shell or Meterpreter session is opened, dedicated interactive session tabs populate automatically.

#### Step 3 — Check the Network Route and Gateway
1. Open a terminal in Kali Linux and inspect your active routing table:
   ```bash
   ip route
   ```
2. Examine the returned output. In our virtual lab environment, the routing output displays:
   ```text
   default via 10.0.2.2 dev eth0
   10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
   ```
3. Technical analysis of the routing entries:
   - `default via 10.0.2.2`: Identifies `10.0.2.2` as the default virtual gateway (VirtualBox NAT gateway).
   - `10.0.2.0/24`: Identifies the directly connected Class C private subnet.
   - `src 10.0.2.15`: Identifies `10.0.2.15` as the local Kali Linux VM's assigned IP address.
> *Target Range Policy:* The default gateway address alone does not define the scan scope. Scanning must be restricted strictly to the instructor-authorized lab network range (e.g., `10.0.2.0/24` or designated target lab VM IP).

#### Step 4 — Run the Host-Discovery Scan through the GUI
1. On the Armitage top menu bar, click **Hosts**.
2. Hover over **Nmap Scan**.
3. Select **Quick Scan** (or **Quick Scan (OS detect)** if available in your version).
4. In the target input dialog prompt, enter the authorized lab subnet or target:
   ```text
   10.0.2.0/24
   ```
5. Click **OK** to launch the scan.
6. Observe the bottom `nmap` console tab as Nmap scans the network in the background.
7. Wait for the scan to finish (indicated by the `msf >` prompt returning).
8. Inspect the main canvas, which dynamically populates with discovered host icons.

#### Step 5 — Examine Discovered Hosts and Services
1. Inspect the discovered host icons on the main canvas:
   - In our actual lab screenshot, the canvas displays three hosts:
     - `10.0.2.2` (Virtual gateway / QEMU virtual NIC)
     - `10.0.2.15` (Kali Linux VM itself)
     - `10.0.2.3` (Virtual DNS service)
2. Left-click a host to select it (e.g., `10.0.2.2`).
3. Right-click the host and select **Services** (or view the recorded output in the bottom `nmap` console tab).
4. Review the discovered listening ports and service names:
   - `10.0.2.2` reports open ports: `135/tcp` (msrpc), `445/tcp` (microsoft-ds), `3000/tcp` (ppp), `5000/tcp` (upnp), `5432/tcp` (postgresql), `8000/tcp` (http-alt).
   - `10.0.2.3` reports open port: `53/tcp` (domain / DNS).
   - `10.0.2.15` (Kali VM): All 100 scanned ports reported in ignored/filtered states.
5. Record the observed IP addresses, port states, and service names.

> *Important Technical Distinction:* An open port indicates an accessible network service responding to connection requests at that timestamp. It is **not proof** that the target is exploitable or vulnerable.

#### Step 6 — Set the Exploit Rank Filter
1. On the top menu bar, click **Attacks**.
2. In the dropdown menu, locate the exploit ranking setting (or module filter options).
3. In our practical workflow, set the exploit rank filter to **Poor** before initiating attack correlation.
4. Understand the technical meaning of the rank filter:
   - Metasploit ranks exploit modules by reliability: *Manual*, *Low*, *Average*, *Normal*, *Good*, *Great*, and *Excellent*.
   - Setting the ranking filter to **Poor** (or *Low*) broadens the search criteria to include modules with lower reliability rankings.
   - **Crucial Distinction:** `Poor` is an exploit module reliability metric (indicating a risk of crashing the service or failing to yield a stable shell); it is **not a vulnerability severity rating** (such as CVSS Critical or High).
   - Lowering the ranking threshold allows Armitage to suggest more experimental candidate modules, but does not guarantee exploit viability.

#### Step 7 — Run "Find Attacks"
1. In the top menu bar, click **Attacks**.
2. Click **Find Attacks**.
3. Observe the progress dialog while Armitage correlates every host's open ports and service banners against Metasploit's database of thousands of exploit modules.
4. When finished, a confirmation dialog appears: *"Attack analysis complete. You will now see an Attack menu when you right-click a host."*
5. Right-click any discovered host on the canvas and observe the newly added **Attack** submenu, categorized by protocol (e.g., `smb`, `http`, `postgres`).

> *Ethical Assessment Boundary:* The appearance of a recommended attack module is simply a correlation suggestion based on port and banner matches. **Do not execute attack modules** unless the instructor has explicitly assigned exploitation against an isolated, deliberately vulnerable laboratory machine (e.g., Metasploitable 2). For this practical, the exercise concludes with reviewing the correlated attack suggestions.

---

### Interface Layout and Visual Observations

```text
+---------------------------------------------------------------------------------------+
| Armitage   View   Hosts   Attacks   Workspaces   Help                                 |
+-----------------------------------+---------------------------------------------------+
| MODULES TREE                      | TARGETS CANVAS                                    |
| > auxiliary                       |   [Monitor Icon]    [Monitor Icon]   [Monitor Icon]   |
| > exploit                         |      10.0.2.2          10.0.2.15        10.0.2.3      |
| > payload                         |   (Gateway/Services)   (Kali Local)     (DNS Host)    |
| > post                            |                                                   |
|                                   |                                                   |
+-----------------------------------+---------------------------------------------------+
| CONSOLE TABS: [Console X] [nmap X]                                                    |
| [*] Nmap: Nmap scan report for 10.0.2.2 -> 135/tcp (msrpc), 445/tcp (microsoft-ds),   |
|     3000/tcp (ppp), 5000/tcp (upnp), 5432/tcp (postgresql), 8000/tcp (http-alt)       |
| [*] Nmap: Nmap scan report for 10.0.2.3 -> 53/tcp (domain)                           |
| [*] Nmap: Nmap done: 256 IP addresses (3 hosts up) scanned in 15.07 seconds           |
| msf >                                                                                 |
+---------------------------------------------------------------------------------------+
```

#### Actual Screenshot Observations
- **Host Icons Present:** `10.0.2.2`, `10.0.2.15`, `10.0.2.3`.
- **Target `10.0.2.2`:** Displayed listening services on ports 135 (msrpc), 445 (microsoft-ds), 3000 (ppp), 5000 (upnp), 5432 (postgresql), and 8000 (http-alt) with QEMU virtual NIC MAC address `52:54:00:12:35:00`.
- **Target `10.0.2.3`:** Displayed port 53 (DNS domain service).
- **Target `10.0.2.15`:** All 100 scanned ports reported in ignored states (local Kali Linux VM).
- **Scan Latency:** Complete sweep of 256 addresses finished in 15.07 seconds.

---

### Exploit Ranking vs. Vulnerability Severity

| Metasploit Exploit Rank | Meaning in Armitage | Contrast with Vulnerability Severity |
| :--- | :--- | :--- |
| **Excellent** | Module is completely reliable, auto-detects target, and never crashes the service. | Rank measures **module reliability and stability**, not threat level. |
| **Great** | Module has auto-detection and handles common service versions stably. | A **Critical** vulnerability (CVSS 9.8) might have a **Manual** or **Poor** exploit rank if the script is unstable. |
| **Good** | Standard exploit with good version coverage; rare service crash risk. | Conversely, a **Low** severity issue might have an **Excellent** rank script. |
| **Normal** | Reliable exploit but requires manual target specification or has edge cases. | *Setting rank to "Poor" simply includes less stable exploit scripts in Armitage's recommendations.* |
| **Average / Poor** | Exploit is unreliable, highly version-specific, or frequently crashes target service. | |

---

### What to Observe
- **Target Visualization:** Hosts appear as visual nodes on the canvas. Uncompromised hosts appear as standard monitors; successfully exploited hosts turn red with lightning bolts.
- **Service Information:** Associating open ports with specific daemon names (e.g., PostgreSQL on 5432, SMB on 445) reveals the network attack surface.
- **Attack Tree Population:** Selecting "Find Attacks" automatically binds potential Metasploit auxiliary and exploit modules to each host's right-click context menu.
- **Safe Practice:** Verification of findings through non-intrusive banner examination is prioritized over aggressive automated exploitation (such as Hail Mary).

---

### Observation for the Lab Record
> **Observation (Armitage Lab Workflow):**  
> Armitage was opened and connected to the Metasploit Framework. The authorised lab network (`10.0.2.0/24`) was examined using the Nmap Scan option. The scan identified three active hosts (`10.0.2.2`, `10.0.2.15`, and `10.0.2.3`) on the canvas. Service inspection on host `10.0.2.2` revealed open ports including 135 (msrpc), 445 (microsoft-ds), and 5432 (postgresql), while `10.0.2.3` exposed port 53 (domain). The exploit rank filter was set to Poor, and the Find Attacks feature was executed to correlate collected service data against Metasploit's exploit database. *(Note: Adjust IP addresses, ports, and host counts to reflect your specific lab environment).*

---

### Result
Graphical network reconnaissance, host discovery, and attack-module mapping were successfully demonstrated using Armitage on the authorized virtual lab network, correlating open service banners with Metasploit exploit modules.

---

### Viva Questions and Answers

1. **What is Armitage?**  
   *Answer:* Armitage is a graphical front end for the Metasploit Framework that visualizes targets, organizes hosts, runs scans, recommends relevant exploit modules, and manages multi-target sessions.
2. **What is Metasploit?**  
   *Answer:* Metasploit is an open-source penetration testing framework used to develop, test, and execute exploit code against remote target systems, supported by a modular database of exploits, payloads, and auxiliary tools.
3. **What is Nmap Scan used for in Armitage?**  
   *Answer:* The Nmap Scan feature in Armitage executes network port scans and operating system detection against target IP ranges directly from the GUI, automatically populating the targets canvas with discovered host nodes and service details.
4. **What does Find Attacks do?**  
   *Answer:* Find Attacks analyzes the open ports and service banners of hosts on the canvas, compares them against Metasploit's module database, and attaches matching exploit modules to each host's right-click context menu.
5. **What is the purpose of a workspace in Armitage/Metasploit?**  
   *Answer:* A workspace isolates host records, scan data, services, and credentials into independent database containers, preventing findings from different assessments or lab exercises from getting mixed together.
6. **What is the difference between an open port and an exploitable vulnerability?**  
   *Answer:* An open port simply means a network service is actively listening and accepting connections. An exploitable vulnerability is a specific software flaw, unpatched bug, or misconfiguration in that service that can be leveraged to execute unauthorized actions or gain access.
7. **What is Meterpreter?**  
   *Answer:* Meterpreter is an advanced, dynamically extensible payload that executes entirely in-memory using DLL injection. It provides an encrypted, interactive communication channel to inspect files, execute commands, capture screenshots, and dump credentials without writing binaries to the target hard drive.

---

### Quick Revision
- **Armitage:** Metasploit Framework GUI client developed by Raphael Mudge.
- **Backend:** Connected via `msfrpcd` to PostgreSQL database (`sudo msfdb status`).
- **Main Panels:** Modules tree (left), Targets canvas (center), Console/nmap tabs (bottom).
- **Network Check:** `ip route` identifies default gateway (`10.0.2.2`) and subnet (`10.0.2.0/24`).
- **Discovery Routine:** `Hosts` $\rightarrow$ `Nmap Scan` $\rightarrow$ `Quick Scan`.
- **Target Analysis:** Right-click host $\rightarrow$ `Services` (inspects ports, protocols, banners).
- **Exploit Rank:** Measures module code reliability (Poor to Excellent), not vulnerability severity.
- **Find Attacks:** Correlates open services with Metasploit exploit modules.
- **Safety Policy:** Prohibit Hail Mary or unauthorized attacks against production systems.

---

## 3. Comparison and Comprehensive Practical Revision

### Overall Tool Comparison

| Metric / Feature | Wireshark | Armitage |
| :--- | :--- | :--- |
| **Category** | Network Packet Analysis & Protocol Sniffing | Attack Management & Metasploit GUI Front End |
| **Primary Scope** | Network Layer 2 through Layer 7 Traffic Inspection | Target Reconnaissance & Vulnerability Correlation |
| **Main Task** | Capture, decode, filter, and inspect network packets | Graphically discover hosts, enumerate services, and find exploits |
| **Input Source** | Promiscuous capture on physical/virtual network NIC (`eth0`) | Metasploit database (`msfdb`) and GUI-driven Nmap sweeps |
| **Main Output** | Packet List, Protocol Tree (Frame, IP, TCP/UDP), `.pcapng` file | Visual host canvas, service tables, and correlated attack menus |
| **Key Filtering** | Display filters (`icmp`, `tcp`, `udp`, `dns`, `http`, `ip.addr`) | Exploit ranking filters (*Poor*, *Average*, *Good*, *Excellent*) |
| **Operational Stance** | Completely passive monitoring (unless generating traffic) | Active reconnaissance (Nmap probes) and attack mapping |

---

### One-Minute Record Summary (Aim, Procedure, Observation, Result)

#### Tool 1: Wireshark
- **Aim:** To capture and inspect live network packets, apply display filters, and analyze protocol layers using Wireshark.
- **Procedure:** Open Wireshark, select interface `eth0`, generate ICMP traffic using `ping 1.1.1.1`, stop capture, apply display filters (`icmp`, `tcp`), inspect packet headers across Frame, Ethernet II, and IPv4 layers, and save capture as `/home/kali/wireshark-practice.pcapng`.
- **Observation:** Wireshark captured live traffic on `eth0`; ping generated ICMP Echo Requests and Replies; packet details displayed MAC addresses under Ethernet II and IP addresses under IPv4; ICMP packets contained Type/Code fields without port numbers; display filters isolated traffic without deleting packets.
- **Result:** Network traffic was successfully captured, filtered, and analyzed at the packet layer using Wireshark.

#### Tool 2: Armitage
- **Aim:** To perform graphical host discovery, service enumeration, and exploit correlation on an authorized virtual network using Armitage.
- **Procedure:** Start Metasploit database (`sudo msfdb status`), launch `sudo armitage`, check network route (`ip route`), run `Hosts` $\rightarrow$ `Nmap Scan` $\rightarrow$ `Quick Scan` on `10.0.2.0/24`, inspect discovered host icons (`10.0.2.2`, `10.0.2.15`, `10.0.2.3`), set exploit rank filter to Poor, and run `Attacks` $\rightarrow$ `Find Attacks`.
- **Observation:** Armitage connected to Metasploit; Nmap scan populated the canvas with 3 active hosts; host `10.0.2.2` showed open ports including 135, 445, and 5432; Find Attacks generated customized attack submenus for discovered services.
- **Result:** Graphical host discovery and automated attack-module correlation were successfully conducted using Armitage on the lab network.

---

### Final Practical Exam Viva Checklist

```text
========================================================================================
PRACTICAL EXAMINATION RAPID-FIRE VIVA CHECKLIST
========================================================================================
1. Q: Why do ICMP ping packets lack source and destination port numbers in Wireshark?
   A: ICMP is a Layer 3 Network protocol encapsulated directly in IP packets (Protocol 1);
      port numbers belong exclusively to Layer 4 Transport protocols like TCP and UDP.

2. Q: What is the main advantage of .pcapng over legacy .pcap?
   A: .pcapng supports multi-interface captures, nanosecond timestamp precision, embedded
      interface metadata, and contextual packet annotations.

3. Q: How does a capture filter differ from a display filter in Wireshark?
   A: Capture filters use BPF syntax before capturing to discard non-matching packets at
      the driver level; display filters use Wireshark syntax post-capture to filter views.

4. Q: What backend services are required for Armitage to operate?
   A: PostgreSQL database service (managed via msfdb) and the Metasploit RPC daemon (msfrpcd).

5. Q: Does the appearance of a module under "Find Attacks" in Armitage prove vulnerability?
   A: No; Find Attacks only matches open port numbers and banner strings to exploit modules.
      Vulnerability must be validated through targeted assessment.

6. Q: What does an exploit rank of "Poor" mean in Armitage?
   A: It indicates a low-reliability exploit module that may crash the target service or fail
      in common environments; it is NOT a measure of vulnerability risk or severity.

7. Q: What is Meterpreter and why is it preferred over a standard command shell?
   A: Meterpreter operates purely in-memory via reflective DLL injection without writing to disk,
      providing stealth, encrypted comms, and rich post-exploitation features (sysinfo, hashdump).
========================================================================================
```
