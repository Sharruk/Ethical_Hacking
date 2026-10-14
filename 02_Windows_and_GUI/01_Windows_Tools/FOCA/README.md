# FOCA (Fingerprinting Organizations with Collected Archives)

## Description
FOCA is an automated Open Source Intelligence (OSINT) tool used to discover publicly available documents on websites and extract hidden metadata. By analyzing file formats such as PDF, DOCX, XLSX, and PPTX, FOCA reveals internal organizational details including filesystem folder structures, software versions, usernames, and network printer queues without directly touching the target's servers.

- **Primary Scope/Domain:** `owasp.org`
- **Official Repository:** [ElevenPaths FOCA Releases](https://github.com/ElevenPaths/FOCA/releases)
- **Detailed Practical Notes:** See [Windows Tools Practical Notes: FOCA](../Windows_Tools.md#1-tool-1-foca-document-metadata-harvesting--osint)

---

## Quick-Start Steps
1. **Create Project:** Open `FOCA.exe` $\rightarrow$ **Project** $\rightarrow$ **New Project** $\rightarrow$ Name: `FOCA-Lab`, Domain: `owasp.org`, Folder: `Documents\FOCA-Lab` $\rightarrow$ Click **Create**.
2. **Search Public Documents:** Select **Google** and **Bing**, click **All** beside Extensions, and click **Search All**.
3. **Download Documents:** Focus the results table, select rows (`Ctrl+A`), right-click $\rightarrow$ **Download All**.
4. **Extract Metadata:** Highlight downloaded documents, right-click $\rightarrow$ **Extract Metadata**.
5. **Inspect Results:** Review the **Metadata Summary** under Folders, Software, Users, Printers, and Files.
6. **Save Log:** Click **Save log to File** in `Documents\FOCA-Lab`.

---

## Lab Record Summary
- **Aim:** To discover publicly available documents associated with a target domain (`owasp.org`) and extract hidden metadata to identify server folders, software versions, usernames, and infrastructure details.
- **Observation:** FOCA was used to search for publicly available documents associated with the selected domain (`owasp.org`). The documents were downloaded and analysed for metadata. The results included folder and software information. The metadata summary showed 17 folders and 8 software entries in this run. One file registered 0 B with a red indicator, demonstrating that failed downloads must be validated before reporting metadata.
- **Result:** Passive document reconnaissance and metadata extraction for `owasp.org` were successfully completed using FOCA v3.4.7.1, identifying internal server folder paths and software version signatures without directly contacting the target web server.

---

> For complete step-by-step procedures, error analysis, viva questions, and revision notes, refer to [../Windows_Tools.md](../Windows_Tools.md#1-tool-1-foca-document-metadata-harvesting--osint).
