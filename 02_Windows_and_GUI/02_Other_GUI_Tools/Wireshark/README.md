# Wireshark (Network Packet Analysis)

## Description
Wireshark is an industry-standard, graphical network protocol analyzer used to capture, decode, and inspect packet transmissions across network interfaces in real time. Operating across all layers of the OSI model, Wireshark allows security analysts to dissect Ethernet frames, IP packet headers, transport-layer segments (TCP/UDP), and ICMP diagnostics, helping detect unencrypted traffic and troubleshoot communication anomalies.

- **Primary Interface:** `eth0` (Kali Linux VM)
- **Official Website:** [Wireshark Official Site](https://www.wireshark.org/)
- **Detailed Practical Notes:** See [Other GUI Tools Practical Notes: Wireshark](../Other_GUI_Tools.md#1-tool-1-wireshark-network-packet-analysis)

---

## Quick-Start Workflow
1. **Launch Wireshark:** Open Wireshark from the Kali application menu (`Sniffing & Spoofing` $\rightarrow$ `Wireshark`).
2. **Select Interface:** Double-click active interface `eth0` displaying packet activity sparklines to begin live capture.
3. **Generate Traffic:** Open a terminal (`Ctrl + Alt + T`) and run `ping 1.1.1.1` (or default gateway `10.0.2.2`). Stop with `Ctrl + C`.
4. **Stop Capture:** Return to Wireshark and click the red square **Stop** button.
5. **Apply Display Filters:** In the filter bar, apply `icmp` to isolate ping traffic, or `tcp`, `udp`, `dns`, `http`.
6. **Inspect Protocol Layers:** In the middle Details pane, expand **Frame**, **Ethernet II** (MACs), **IPv4** (IPs), and **ICMP** (Type/Code). Notice ICMP has **no port numbers**.
7. **Save & Reopen:** Save as `/home/kali/wireshark-practice.pcapng` via **File** $\rightarrow$ **Save As**, close, and reopen via **File** $\rightarrow$ **Open** to verify.

---

## Main Concepts to Observe
- **Three Panes:** Packet List (top), Packet Details (middle protocol tree), Packet Bytes (raw hex and ASCII).
- **Protocol Dissection:** Ethernet II handles Layer 2 physical addressing; IPv4 handles Layer 3 logical routing; TCP/UDP handle Layer 4 ports.
- **ICMP Feature:** Operates directly over IP (Protocol 1) and does **not contain TCP or UDP port numbers**.
- **Display Filters:** Non-destructive view filters (`icmp`, `tcp`, `udp`, `dns`, `http`, `ip.addr == <IP>`, `tcp.port == <Port>`).
- **File Format:** `.pcapng` (PCAP Next Generation) preserves multi-interface metadata and packet structures.

---

## Lab Record Summary
- **Aim:** To capture, decode, and analyze live network packets on an active network interface using Wireshark, inspect multi-layer protocol headers of generated ICMP traffic, apply display filters, and manage capture sessions.
- **Observation:** Wireshark was used to capture network traffic on the selected interface (`eth0`). ICMP traffic was generated using the `ping` command and captured. Packets were inspected to identify protocol details, source and destination addresses, and other available fields. Display filters (`icmp`, `tcp`, `udp`) were applied to isolate specific protocols without altering the underlying capture. The capture was saved as a `.pcapng` file and reopened for verification.
- **Result:** Network traffic on the virtual interface was successfully captured, filtered, and analyzed at the protocol layer using Wireshark, and persistence was verified by reopening the saved `.pcapng` capture file.

---

> For complete step-by-step procedures, filter syntax, viva questions, and exam revision, refer to [../Other_GUI_Tools.md](../Other_GUI_Tools.md#1-tool-1-wireshark-network-packet-analysis).
