# theHarvester (OSINT Email & Subdomain Enumeration)

## Introduction
`theHarvester` is a Python-based Open Source Intelligence (OSINT) gathering utility designed for the early reconnaissance phase of a penetration test. It harvests publicly exposed corporate email addresses, employee names, virtual hostnames, subdomains, open ports, and banners from public search engines, PGP key servers, and threat intelligence databases without transmitting active packets to the target's servers.

- **Package:** `theharvester`
- **Reconnaissance Type:** Passive OSINT (Certificate Transparency logs, Google, Bing, HackerTarget, CRT.sh)
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: theHarvester](../Terminal_Tools.md#6-topic-5-theharvester-osint-email--subdomain-enumeration)

---

## Installation & Verification
```bash
sudo apt update
sudo apt install theharvester -y
theHarvester --version
theHarvester -h
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `theHarvester -d <authorized-domain> -b <source>` | Queries the specified data source/search engine for public intelligence on the domain. |
| `theHarvester -d example.com -b google` | Collects public email addresses and hostnames indexed by the Google search engine. |
| `theHarvester -d example.com -b crtsh` | Queries the crt.sh Certificate Transparency logs to discover valid registered subdomains. |
| `theHarvester -d example.com -b crtsh,hackertarget -l 20` | Queries multiple public sources simultaneously while capping the query results to 20 items. |
| `theHarvester -d microsoft.com -b all -l 300 -q` | Searches across all available sources up to 300 results in quiet mode (suppressing banners). |

---

## Practical Notes & Backend Selection
> [!TIP]
> - Search engines like Google or Bing frequently enforce bot CAPTCHAs or rate limits (HTTP 429).
> - For reliable lab demonstrations without API keys, prefer Certificate Transparency logs: `-b crtsh` or `-b certspotter`.
> - Check available backends anytime using `theHarvester -h`.
> - Zero search results do not prove subdomains do not exist; it indicates that the selected public search engine has no indexed entries.

---

## Observation Template for Lab Record
> **Observation:** theHarvester was used to collect publicly available information associated with the selected domain. The results were examined for email addresses, subdomains, hostnames and IP addresses where available. Target domain `[authorized-domain]` was queried across sources `[observed sources, e.g., crtsh, hackertarget]`, successfully identifying `[number]` subdomains `[e.g., mail.example.com, vpn.example.com]` and `[number]` publicly indexed email addresses.

---

> For API key configuration (`api-keys.yaml`), proxy routing, and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#6-topic-5-theharvester-osint-email--subdomain-enumeration).
