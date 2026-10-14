# Nmap (Network Discovery & Port Scanning)

## Introduction
Nmap (Network Mapper) is an industry-standard open-source network auditing and security scanning tool. It performs active network reconnaissance by transmitting raw IP, TCP, UDP, and ICMP packets to discover live hosts, determine open network ports, fingerprint listening service applications and version banners, and infer target operating systems.

- **Package:** `nmap`
- **Scanning Types:** Host Discovery (`-sn`), TCP SYN Scan (`-sS`), Version Detection (`-sV`), OS Fingerprinting (`-O`), Aggressive Scan (`-A`)
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: Nmap](../Terminal_Tools.md#5-topic-4-nmap-network-discovery--port-scanning)

---

## Installation & Verification
```bash
sudo apt update
sudo apt install nmap -y
nmap --version
nmap -h
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `nmap -sn <authorized-lab-subnet>` | Performs host discovery across the subnet using ping scans without probing TCP/UDP ports. |
| `nmap <authorized-target-IP>` | Scans the top 1,000 common TCP ports on the target host to determine port state. |
| `nmap -p 22,80,443,587 <authorized-target-IP>` | Checks only the specified comma-separated TCP ports on the designated target. |
| `nmap -p- <authorized-target-IP>` | Scans all 65,535 TCP ports on the target to identify services on non-standard ports. |
| `sudo nmap -sS <authorized-target-IP>` | Executes a stealthy TCP SYN (half-open) scan by tearing down handshakes with RST packets. |
| `sudo nmap -sS -sV -O <authorized-target-IP>` | Combines TCP SYN scanning, application service banner detection, and OS fingerprinting. |
| `sudo nmap -A <authorized-target-IP>` | Enables aggressive scanning including OS detection, service versioning, script scanning, and traceroute. |

---

## Lab Manual Errata & Safety Guidance
> [!IMPORTANT]
> - **Port Syntax Correction:** The manual contains the typo `nmap -p- 22,80,443,587`. The flag `-p-` instructs Nmap to scan *all 65,535 ports*. To scan a custom port list, omit the trailing hyphen: `nmap -p 22,80,443,587 <target>`.
> - **Version Check Correction:** Use `nmap --version` (or `nmap -V`) rather than `nmap --h`.
> - **Authorization:** Nmap sends active packets that trigger firewalls and IDS/IPS alerts. Scan only your assigned laboratory IP address or virtual lab subnet.

---

## Observation Template for Lab Record
> **Observation:** Nmap was used to discover hosts and examine the network ports of the authorised laboratory target. The scan output was reviewed to identify host status, open ports and available service information. Host discovery on subnet `[authorized-lab-subnet]` confirmed `[number]` active hosts. A comprehensive TCP scan on target `[authorized-target-IP]` identified open ports: `[e.g., 22/tcp open ssh OpenSSH 8.2p1, 80/tcp open http Apache httpd 2.4.41]`. OS fingerprinting matched `[observed OS candidate, e.g., Linux 5.4]` with high confidence.

---

> For detailed TCP flag state tables, NSE script usage, timing templates (`-T4`), and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#5-topic-4-nmap-network-discovery--port-scanning).
