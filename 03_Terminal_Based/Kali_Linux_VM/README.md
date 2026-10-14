# Kali Linux Virtual Machine

## Introduction
Kali Linux is a Debian-based Linux distribution pre-configured with hundreds of security auditing and penetration testing tools. In this laboratory, Kali Linux acts as the analyst workstation operating inside Oracle VirtualBox to execute network discovery, OSINT, and vulnerability assessment tasks in an isolated virtual lab environment.

- **Platform:** Oracle VirtualBox (Debian 64-bit)
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: Kali Linux VM](../Terminal_Tools.md#2-topic-1-kali-linux-virtual-machine)

---

## Setup & Verification Procedure
1. **VM Configuration:** Create VirtualBox VM named `Kali Linux`, type Linux / Debian (64-bit), 2048–4096 MB RAM, 2 vCPUs, and 20 GB VDI virtual disk.
2. **Installation:** Boot from official Kali Linux ISO, select **Graphical Install**, set user/password (`kali`/`kali`), and install GRUB to `/dev/sda`.
3. **Network Mode:** Configure VirtualBox network adapter as NAT (receives `10.0.2.15`) or Host-Only (e.g., `192.168.56.0/24`).

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `cat /etc/os-release` | Displays the installed operating system distribution, release version, and build name. |
| `ip a` | Lists all active network interfaces, hardware MAC addresses, and assigned IP configurations. |
| `ip route` | Displays the kernel network routing table and the active default gateway. |
| `ping -c 4 1.1.1.1` | Sends four ICMP echo requests to test basic outbound network connectivity. |

---

## Observation Template for Lab Record
> **Observation:** Kali Linux was installed and started successfully in VirtualBox. The operating system and network configuration were checked using terminal commands. The assigned interface, IP address and routing information were examined. Command `cat /etc/os-release` confirmed Kali Linux Debian-based distribution, `ip a` identified interface `[observed interface, e.g., eth0]` with IP `[observed IP, e.g., 10.0.2.15]`, and `ip route` confirmed default gateway `[observed gateway, e.g., 10.0.2.2]`.

---

> For complete step-by-step installation guides, network adapter mode comparisons, and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#2-topic-1-kali-linux-virtual-machine).
