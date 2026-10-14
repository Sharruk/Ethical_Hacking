# WHOIS (Domain & IP Registration Lookup)

## Introduction
WHOIS is an Open Source Intelligence (OSINT) protocol and command-line tool used to query regional internet registries and domain registrars. It retrieves publicly available registration details, domain creation/expiration dates, registrar information, and authoritative nameservers without interacting directly with the target host.

- **Protocol:** WHOIS (TCP Port 43)
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: WHOIS](../Terminal_Tools.md#3-topic-2-whois-domain--ip-registration-lookup)

---

## Installation & Setup
```bash
sudo apt update
sudo apt install whois -y
whois --version
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `whois example.com` | Queries registry databases for public domain registration, registrar, and nameserver details. |
| `whois 8.8.8.8` | Retrieves Autonomous System Number, network CIDR range, and ISP ownership for the IP address. |
| `whois -h whois.verisign-grs.com example.com` | Queries a specific authoritative registry host directly instead of using automated referral redirects. |
| `whois --help` | Displays the help manual, syntax format, and available operational flags. |

---

## Observation Template for Lab Record
> **Observation:** WHOIS was used to retrieve the available registration information for the specified domain or IP address. The output was examined for registrar details, registration dates, nameservers and registration status where available. Querying `[target domain / IP]` revealed registrar `[observed registrar]`, creation date `[observed creation date]`, expiration date `[observed expiration date]`, and authoritative name servers `[observed name servers]`.

---

> For complete command references, GDPR redaction explanations, and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#3-topic-2-whois-domain--ip-registration-lookup).
