# Netdiscover (Local ARP Network Reconnaissance)

## Introduction
Netdiscover is an active and passive Address Resolution Protocol (ARP) reconnaissance tool specifically designed for wireless and switched Ethernet local area networks (LANs). Because ARP operates at the Data Link Layer (Layer 2) and is not blocked by host-level personal firewalls (such as Windows Defender Firewall), Netdiscover reliably detects active hosts, identifies duplicate IP assignments, and resolves Network Interface Card (NIC) hardware vendors via OUI lookups.

- **Package:** `netdiscover`
- **Protocol:** ARP (Address Resolution Protocol - Layer 2)
- **Privilege Requirement:** Requires root / `sudo` privileges to transmit and capture raw Ethernet frames.
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: Netdiscover](../Terminal_Tools.md#7-topic-6-netdiscover-local-arp-network-reconnaissance)

---

## Installation & Setup
```bash
sudo apt update
sudo apt install netdiscover -y
netdiscover -h
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `sudo netdiscover` | Actively broadcasts ARP requests on the default network interface to discover live LAN hosts. |
| `sudo netdiscover -r <authorized-lab-subnet>` | Scans the specified CIDR network range (e.g., `10.0.2.0/24`) by generating sequential ARP requests. |
| `sudo netdiscover -i eth0` | Binds the ARP discovery process explicitly to the specified network interface card (`eth0`). |
| `sudo netdiscover -i wlan0 -r <authorized-lab-subnet>` | Directs the ARP discovery scan over the designated wireless adapter interface (`wlan0`). |
| `sudo netdiscover -p` | Enables passive listening mode, capturing unsolicited broadcast ARP frames without transmitting packets. |

---

## Network Mode Warning & Subnet Verification
> [!WARNING]
> Before running Netdiscover, always verify your virtual network configuration using `ip a` and `ip route`:
> - **VirtualBox NAT Mode:** Typically uses `10.0.2.0/24`. Only the gateway and DHCP services respond.
> - **VirtualBox Host-Only Mode:** Typically uses `192.168.56.0/24`. Inter-VM communication is allowed.
> - **Bridged Adapter Mode:** Bridges directly to the physical LAN router.
> **Never** execute broad ARP scans like `192.168.1.0/24` or `192.168.0.0/24` on public Wi-Fi or college campus networks without authorization.

---

## Observation Template for Lab Record
> **Observation:** Netdiscover was used to identify live hosts in the authorised local laboratory network. The output was examined to record the available IP addresses, MAC addresses and related host information. An active scan on interface `[observed interface, e.g., eth0]` over subnet `[authorized-lab-subnet]` captured `[number]` live host ARP responses. Target VM IP `[observed IP]` responded from MAC address `[observed MAC]`, identified as hardware vendor `[e.g., PCS Systemtechnik GmbH / Oracle VirtualBox]`.

---

> For passive vs active ARP comparison, keyboard controls (`q`, `s`), and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#7-topic-6-netdiscover-local-arp-network-reconnaissance).
