# Armitage (GUI Front End for Metasploit)

## Description
Armitage is a graphical cyber attack management tool developed by Raphael Mudge for the Metasploit Framework. It visualizes targets on an interactive canvas, organizes hosts by operating system, recommends applicable exploit modules, and manages multi-target penetration testing sessions. Armitage connects directly to the Metasploit RPC daemon (`msfrpcd`) backed by a PostgreSQL database (`msfdb`), streamlining reconnaissance and vulnerability correlation into an intuitive point-and-click interface.

- **Primary Target/Subnet:** `10.0.2.0/24` (Authorized Lab Network)
- **Official Website:** [Armitage - Fast and Easy Hacking](http://www.fastandeasyhacking.com/)
- **Detailed Practical Notes:** See [Other GUI Tools Practical Notes: Armitage](../Other_GUI_Tools.md#2-tool-2-armitage-gui-front-end-for-metasploit)

---

## Quick-Start Workflow
1. **Verify Database:** Open terminal and run `sudo msfdb status`. Start if stopped (`sudo msfdb start`).
2. **Launch Armitage:** Run `sudo armitage` and click **Connect** on the default RPC prompt (`127.0.0.1:55553`).
3. **Check Routing:** Run `ip route` in terminal to confirm the local gateway (`10.0.2.2`) and subnet (`10.0.2.0/24`).
4. **Run Discovery Scan:** In Armitage, click **Hosts** $\rightarrow$ **Nmap Scan** $\rightarrow$ **Quick Scan**, enter the authorized lab range (`10.0.2.0/24`), and click OK.
5. **Inspect Discovered Hosts:** Review the canvas icons (`10.0.2.2`, `10.0.2.15`, `10.0.2.3`). Right-click `10.0.2.2` $\rightarrow$ **Services** to view open ports (135 msrpc, 445 microsoft-ds, 5432 postgresql).
6. **Set Exploit Rank:** Click **Attacks** and configure the ranking filter to **Poor** (expands module candidate pool).
7. **Find Attacks:** Click **Attacks** $\rightarrow$ **Find Attacks**. When complete, right-click any host to view the populated protocol-specific **Attack** submenus.

---

## Main Concepts to Observe
- **Four Interface Panels:** Modules tree (left), Targets canvas (center), Console/nmap tabs (bottom), Session tabs (dynamic).
- **Service vs. Vulnerability:** An open port confirms a listening service; it is **not proof** that the target is exploitable.
- **Exploit Rank vs. Severity:** Exploit rank (*Poor* to *Excellent*) measures **exploit code reliability**, not CVSS vulnerability severity.
- **Find Attacks Feature:** Analyzes host open ports and banners to attach matching Metasploit modules to context menus.
- **Safety Boundary:** Never run automated exploitation (Hail Mary) or unauthorized attacks against unapproved targets.

---

## Lab Record Summary
- **Aim:** To perform graphical host discovery, service enumeration, and attack-module analysis on an authorized virtual lab network using Armitage, the GUI front end for the Metasploit Framework.
- **Observation:** Armitage was opened and connected to the Metasploit Framework. The authorised lab network (`10.0.2.0/24`) was examined using the Nmap Scan option. The discovered hosts (`10.0.2.2`, `10.0.2.15`, and `10.0.2.3`) and their available service information were reviewed. Host `10.0.2.2` showed open ports including 135 (msrpc), 445 (microsoft-ds), and 5432 (postgresql), while `10.0.2.3` exposed port 53 (domain). The exploit rank filter was set to Poor, and the Find Attacks feature was used to identify potentially relevant modules based on the collected information.
- **Result:** Graphical network reconnaissance, host discovery, and attack-module mapping were successfully conducted on the authorized lab network using Armitage, correlating open service banners with Metasploit exploit modules.

---

> For complete step-by-step procedures, screenshot analysis, viva questions, and exam revision, refer to [../Other_GUI_Tools.md](../Other_GUI_Tools.md#2-tool-2-armitage-gui-front-end-for-metasploit).
