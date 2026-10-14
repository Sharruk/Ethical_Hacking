# nslookup (DNS Query & Record Resolution)

## Introduction
`nslookup` (Name Server Lookup) is a network administration command-line utility used to query the Domain Name System (DNS) to resolve domain names to IP addresses and examine specific resource records such as Mail Exchangers (MX), Name Servers (NS), and reverse pointers (PTR).

- **Package:** `dnsutils` (or `bind9-dnsutils`)
- **Protocol:** DNS (UDP/TCP Port 53)
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: nslookup](../Terminal_Tools.md#4-topic-3-nslookup-dns-query--record-resolution)

---

## Installation & Setup
```bash
sudo apt update
sudo apt install dnsutils -y
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `nslookup example.com` | Performs a standard forward DNS lookup, resolving the domain name to its IPv4 (A) or IPv6 (AAAA) addresses. |
| `nslookup -type=mx example.com` | Queries Mail Exchange (MX) resource records, identifying servers responsible for domain email. |
| `nslookup -type=ns example.com` | Queries Name Server (NS) records, identifying the authoritative DNS servers for the zone. |
| `nslookup example.com 8.8.8.8` | Queries the target domain explicitly using the specified external DNS server (`8.8.8.8`). |
| `nslookup 8.8.8.8` | Executes a reverse DNS lookup mapping an IP address back to its canonical hostname via PTR records. |

---

## Observation Template for Lab Record
> **Observation:** nslookup was used to resolve the specified domain and query DNS records. The available IP addresses, mail servers and name servers were examined, and the results were recorded. For target domain `[target domain]`, forward resolution returned IP `[observed IP]`. Querying `-type=mx` identified mail servers `[observed MX servers]`, while `-type=ns` identified authoritative nameservers `[observed nameservers]`. A reverse query on `[target IP]` resolved to `[observed reverse hostname]`.

---

> For interactive lookup prompts, BIND version notes, and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#4-topic-3-nslookup-dns-query--record-resolution).
