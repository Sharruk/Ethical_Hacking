# Nikto (Web Server Vulnerability Scanner)

## Introduction
Nikto is an industry-standard open-source command-line web server assessment scanner. It inspects web servers for potentially dangerous files and CGIs, outdated server software and plugins, insecure server configuration items, default scripts, and missing HTTP security headers.

- **Package:** `nikto`
- **Scanner Type:** Active Web Server Vulnerability & Misconfiguration Scanner
- **Detailed Practical Notes:** See [Terminal Tools Practical Notes: Nikto](../Terminal_Tools.md#9-topic-8-nikto-web-server-vulnerability-scanner)

---

## Installation & Setup
```bash
sudo apt update
sudo apt install nikto -y
nikto -Version
```

---

## Essential Commands

| Command | One-Line Explanation |
| :--- | :--- |
| `nikto -h <authorized-lab-web-target>` | Executes a comprehensive security and configuration audit against the designated web server. |
| `nikto -h <authorized-lab-web-target> -p 443 -ssl` | Connects over an encrypted HTTPS SSL/TLS channel on port 443 to audit the web server. |
| `nikto -h <authorized-lab-web-target> -o report.txt` | Saves the complete vulnerability and misconfiguration scan findings to a plain text file. |
| `nikto -h <authorized-lab-web-target> -o report.html -Format html` | Exports a structured, styled HTML vulnerability report for laboratory documentation. |
| `nikto -h <authorized-lab-web-target> -C all` | Instructs Nikto to execute all available CGI vulnerability check scripts against the target. |

---

## Laboratory Target Safety & Finding Interpretation
> [!CAUTION]
> - Nikto is an **active scanner** that transmits thousands of HTTP GET/POST requests containing known exploit payloads and suspicious URI paths.
> - These scans generate prominent server error log entries (HTTP 404, 500) and immediately trigger Web Application Firewalls (WAF) and IDS/IPS alarms.
> - Run Nikto **only** against designated laboratory targets (e.g., local Damn Vulnerable Web Application [DVWA], Metasploitable2 / Metasploitable3, or authorized training IPs).
> - A Nikto finding indicates a potential risk or misconfiguration that requires manual verification; it does not automatically prove exploitability.

---

## Common Findings Checklist
- **Server Header Disclosure:** Web server version banner exposed (e.g., `Apache/2.4.41 (Ubuntu)`).
- **Missing Anti-Clickjacking Header:** Absence of `X-Frame-Options` allows embedding in malicious `<iframe>` tags.
- **Missing MIME Sniffing Protection:** Absence of `X-Content-Type-Options: nosniff`.
- **Default / Sensitive Files:** Presence of `/admin/`, `/phpinfo.php`, `/test.php`, or `/config.inc`.

---

## Observation Template for Lab Record
> **Observation:** Nikto was used to scan the authorised laboratory web server for potential issues and misconfigurations. The output was examined for server information, outdated software warnings, accessible files and security-header findings. Scanning target `[authorized-lab-web-target]` identified web server software `[observed banner, e.g., Apache/2.4.41]`. Several missing security headers were flagged (`X-Frame-Options`, `X-Content-Type-Options`), and `[number]` potentially sensitive paths `[e.g., /icons/README, /login.php]` were located. The complete vulnerability report was exported to `[report.txt / report.html]`.

---

> For tuning options (`-Tuning`), plugin selection, authentication scans (`-id`), and viva questions, refer to [../Terminal_Tools.md](../Terminal_Tools.md#9-topic-8-nikto-web-server-vulnerability-scanner).
