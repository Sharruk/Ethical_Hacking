# Maltego Community Edition

## Description
Maltego is an interactive visual link-analysis platform designed for Open Source Intelligence (OSINT) and digital footprinting. It maps digital assets and relationships as an interactive visual graph. By executing automated queries called "transforms," Maltego discovers linked subdomains, email addresses, and server IP infrastructure from a single starting domain entity.

- **Primary Scope/Domain:** `ssn.edu.in`
- **Official Website:** [Maltego Community Edition](https://www.maltego.com/)
- **Detailed Practical Notes:** See [Windows Tools Practical Notes: Maltego](../Windows_Tools.md#2-tool-2-maltego-visual-link-analysis--osint-infrastructure-mapping)

---

## Quick-Start Steps
1. **New Graph:** Open Maltego $\rightarrow$ **File** $\rightarrow$ **New Graph**.
2. **Add Domain Entity:** From Entity Palette, drag **Domain** onto the canvas $\rightarrow$ double-click $\rightarrow$ enter `ssn.edu.in`.
3. **Discover DNS Names:** Right-click `ssn.edu.in` $\rightarrow$ **Run Transform** $\rightarrow$ search `dns` $\rightarrow$ select `[Utilities] To DNS Name (interesting) [SecurityTrails]` (or `To DNS Name - [DNS]`) $\rightarrow$ click **Run**.
4. **Discover Email Addresses:** Right-click `ssn.edu.in` $\rightarrow$ **Run Transform** $\rightarrow$ search `email` $\rightarrow$ select `To Email Address` transform $\rightarrow$ click **Run**.
5. **Find IP Infrastructure:** Select a discovered DNS Name entity (e.g., `mail.somca.ssn.edu.in`) $\rightarrow$ right-click $\rightarrow$ **Run Transform** $\rightarrow$ search `IP` $\rightarrow$ select `To IP Address [DNS]` $\rightarrow$ click **Run**.
6. **Analyze Graph:** Review the visual network mapping domain $\rightarrow$ subdomains $\rightarrow$ emails $\rightarrow$ IP addresses.

---

## Lab Record Summary
- **Aim:** To construct a visual Open Source Intelligence (OSINT) relationship graph for the target domain `ssn.edu.in` using Maltego Community Edition, mapping its subdomains, email infrastructure, and network IP addresses through automated transforms.
- **Observation:** Maltego was used to create a Domain entity for `ssn.edu.in`. DNS and email transforms were executed to discover related entities. An IP-address transform was then run on a DNS-name entity. The resulting graph displayed the relationships between the domain, the discovered DNS names, available email addresses and related IP addresses.
- **Result:** Visual OSINT relationship mapping for `ssn.edu.in` was successfully conducted using Maltego Community Edition, generating an interactive graph connecting domain assets, subdomains, and host IP addresses without active target probing.

---

> For complete step-by-step procedures, transform explanations, viva questions, and revision notes, refer to [../Windows_Tools.md](../Windows_Tools.md#2-tool-2-maltego-visual-link-analysis--osint-infrastructure-mapping).
