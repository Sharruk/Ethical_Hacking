# Sublist3r (Search Engine Subdomain Enumeration)

## Introduction
Sublist3r is an open-source Python OSINT reconnaissance tool designed to enumerate subdomains of websites using search engines and public threat intelligence services. It scrapes results from engines like Google, Yahoo, Bing, Baidu, and Ask, as well as intelligence platforms like Netcraft, VirusTotal, ThreatCrowd, DNSdumpster, and ReverseDNS, helping security analysts map an organization's external attack surface.

- **Package:** `sublist3r`
- **Engine Dependencies:** Scrapes public search engine result pages (SERPs)
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: Sublist3r](../Terminal_Tools.md#8-topic-7-sublist3r-search-engine-subdomain-enumeration)

---

## Installation & Setup
```bash
sudo apt update
sudo apt install sublist3r -y
sublist3r --help
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `sublist3r -d <authorized-domain>` | Queries supported search engines to harvest public subdomains for the specified target. |
| `sublist3r -d microsoft.com` | Enumerates subdomains for the target domain using all default search engines. |
| `sublist3r -d microsoft.com -o results.txt` | Executes subdomain enumeration and saves all discovered hosts to a formatted text file. |
| `cat results.txt` | Displays the saved subdomain enumeration findings in the terminal. |
| `sublist3r -d example.com -e google,bing` | Restricts subdomain enumeration queries specifically to Google and Bing engines. |

---

## Operational Notes & Troubleshooting
> [!NOTE]
> - Sublist3r relies on public search engine HTML scrapers. Search engines may temporarily block automated scrapers with CAPTCHAs or HTTP 429 errors.
> - If an engine blocks queries, isolate functional engines with `-e bing,baidu` or use certificate transparency tools like `theHarvester -b crtsh`.
> - Always verify that your target domain is entered without URL schemes (use `example.com`, not `https://example.com`).

---

## Observation Template for Lab Record
> **Observation:** Sublist3r was used to enumerate publicly available subdomains associated with the specified domain. The discovered subdomains were displayed and saved to a text file for review. Target domain `[authorized-domain]` was processed across search engines `[observed engines]`. A total of `[number]` unique subdomains were discovered `[e.g., mail.domain.com, vpn.domain.com, portal.domain.com]` and exported to file `[results.txt]`.

---

> For search engine scraping mechanics, port-scanning flags (`-p`), and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#8-topic-7-sublist3r-search-engine-subdomain-enumeration).
