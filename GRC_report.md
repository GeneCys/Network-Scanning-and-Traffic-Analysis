# GRC Report — Exercise 2: Network Scanning and Traffic Analysis

- **Author:** Gene Crittenden
- **Date:** 2025-11-13
- **Environment:** VirtualBox Host-Only SOC Lab
- **Exercise Reference:** Exercise 2 — Network Scanning and Traffic Analysis

---

**Overview**

This GRC report links the technical activities performed during Exercise 2 to relevant governance, risk, and compliance (GRC) controls and objectives.

The exercise demonstrated the detection and analysis of network reconnaissance activity using **Wireshark** and **Nmap** in a controlled, isolated lab.
The purpose of this report is to document how these activities align with organizational monitoring, incident response, and compliance requirements.

---

**Governance Context**

Network scanning and reconnaissance detection are essential components of an organization’s **security monitoring and assurance program**.
They help validate that:

- Logging and monitoring tools are correctly capturing network events.
- Analysts can identify abnormal or suspicious network behavior.
- Technical controls support governance policies related to **threat detection**, **incident response**, and **continuous monitoring**.

This aligns with strategic cybersecurity governance objectives such as **proactive threat detection** and **continuous improvement of security operations**.

---

**Compliance Framework Mapping**

| Framework | Control ID | Control Description | Relevance to Exercise |
|------------|-------------|---------------------|------------------------|
| **NIST SP 800-53 Rev.5** | SI-4 | *Information System Monitoring* | Demonstrates monitoring and analysis of reconnaissance activity at the network level. |
| **NIST SP 800-61r2** | IR-4 | *Incident Handling* | Simulates identification and analysis stages of the incident response process. |
| **ISO/IEC 27001:2022** | A.12.4.1 | *Event Logging and Monitoring* | Reinforces the importance of continuous monitoring for potential intrusion activity. |
| **CIS Controls v8** | 8.1 | *Establish and Maintain an Audit Log Management Process* | Uses Wireshark logs and network captures as part of network audit and analysis processes. |
| **CIS Controls v8** | 13.2 | *Perform Active Discovery of Assets* | Nmap scanning demonstrates asset discovery and exposure testing within a controlled environment. |

---

**Risk Management Considerations**

| Risk Area | Description | Mitigation |
|------------|-------------|-------------|
| **Reconnaissance Risk** | Attackers use network scanning to map exposed systems and services. | Implement monitoring and IDS tools to detect and alert on scanning behavior. |
| **Exposure of Unnecessary Services** | Open or unmonitored ports may increase attack surface. | Regular port and service audits using Nmap; restrict services via firewalls. |
| **Insufficient Visibility** | Lack of packet capture or monitoring may delay detection. | Deploy network monitoring solutions and baseline normal traffic patterns. |
| **Human Error in Response** | Misinterpretation of scanning behavior could lead to false positives. | Provide SOC analyst training and define clear triage procedures. |

---

**Lessons Learned (GRC Perspective)**

- Regularly simulate reconnaissance activity to validate monitoring effectiveness.
- Align detection processes with organizational incident response plans.
- Maintain logs and captures as evidence for compliance audits.
- Use findings to inform **continuous monitoring policy reviews** and **risk register updates**.

---

**Supporting Documentation**

| File | Description |
|------|-------------|
| `incident_report.md` | Technical incident analysis and detection evidence |
| `README.md` | Exercise overview and procedure documentation |
| `nmap_scan.pcap` | Captured scan traffic for evidence and review |
| `nmap_scan_commands_1.png` | Screenshot of Nmap commands used for reconnaissance |
| `nmap_scan_commands_2.png` | Screenshot of Nmap commands used for reconnaissance |
| `wireshark_filters_1.png` | Screenshot of wireshark filters used for analysis |
| `wireshark_filters_2.png` | Screenshot of wireshark filters used for analysis |
| `grc_exercise.md` | This file — GRC mapping and risk perspective |

---

**Future Improvements**

- Integrate IDS/IPS tools (e.g., **Suricata** or **Zeek**) for alerting and automated detection.
- Create policies defining **authorized network scanning procedures** to ensure compliance.
- Implement **SIEM correlation rules** for real-time detection of reconnaissance events.
- Periodically review GRC mappings against updated standards (NIST, ISO, CIS).

---

**End of GRC Report**
