# Incident Report — Exercise 2: Network Scanning and Traffic Analysis

**Incident ID:** SOC-002
**Date/Time:** 2025-11-13
**Reported By:** Gene Crittenden
**Environment:** Isolated VirtualBox host-only network
**Systems Involved:**
- `client` VM (192.168.56.102) — Nmap scanner
- `web-server` VM (192.168.56.101) — nginx target

---

## 1. Summary

This exercise simulated a network reconnaissance event in a controlled SOC lab.
An Nmap scan was launched from the `client` VM against the `web-server` to identify open ports and services.
Wireshark was used on the `web-server` to capture and analyze the resulting network traffic.

The exercise demonstrates how network scanning activity appears in packet captures and how SOC analysts can detect and interpret early-stage reconnaissance attempts.

---

## 2. Event Details

| Field | Description |
|-------|-------------|
| **Source IP** | 192.168.56.11 (`client` VM) |
| **Destination IP** | 192.168.56.10 (`web-server` VM) |
| **Method Used** | Nmap TCP connect and service scans |
| **Commands Executed** | `nmap -sn 192.168.56.1/24`<br>`nmap -sT -p 1-1024 192.168.56.101`<br>`nmap -sV 192.168.56.101` |
| **Protocols Observed** | ARP, TCP (SYN, SYN-ACK, RST), HTTP |
| **Captured Using** | Wireshark on the web-server VM |
| **Files** | `normal_traffic.pcap`<br>`nmap_scan.pcap` |

---

## 3. Analysis

**Observation:**
- The Nmap scan generated numerous TCP SYN packets to sequential ports (1–1024).
- The web server responded with SYN-ACK packets for open ports and RST packets for closed ports.
- Wireshark revealed clear indicators of a port scan pattern: repetitive SYN packets, increasing port numbers, and uniform source timing.
- The only open port detected was TCP/80 (nginx web server).

**Indicators of Scanning:**
- High frequency of SYN packets from a single host.
- Sequential port targeting with minimal time between probes.
- No application-layer traffic except a few HTTP requests for open ports.

**Impact:**
- No damage or data loss (isolated environment).
- Demonstrates how scanning appears to defenders monitoring live networks.

**Severity:** Low (training simulation only)
**Category:** Network Reconnaissance / Scanning

---

## 4. SOC Recommendations

1. Implement **intrusion detection system (IDS)** rules to detect SYN scan patterns.
2. Enable **network flow monitoring** to capture traffic anomalies and port sweeps.
3. Configure **firewall rate limits** or **port knocking** to slow down active scans.
4. Maintain regular Wireshark or Zeek captures in lab scenarios for training analysis.
5. Educate analysts to correlate scan traffic with server logs for complete context.

---

## 5. GRC Framework Mapping

| Framework | Control | Relevance |
|-----------|----------|-----------|
| **NIST SP 800-53** | SI-4: Information System Monitoring | Continuous monitoring detects early reconnaissance and scanning activity. |
| **ISO 27001:2022** | A.12.4.1: Event Logging | Logging and analysis of network activity support incident detection. |
| **CIS Controls v8** | 8.1: Establish and Maintain Audit Logs | Demonstrates effective collection and review of network telemetry for SOC operations. |

---

## 6. Actions Taken

- Nmap scans executed in a controlled environment.
- Wireshark captures created and analyzed for network reconnaissance signatures.
- Key packets filtered and exported for evidence.
- Incident findings documented in this report and associated Markdown files.

---

## 7. Lessons Learned

- Even basic Nmap scans leave a distinct network footprint detectable via packet analysis.
- SOC teams can reliably detect reconnaissance by recognizing SYN flood patterns.
- Captured data supports both technical (SOC) and compliance (GRC) documentation.
- Combining Nmap and Wireshark provides valuable training for real-world incident detection and reporting.

---

## 8. Supporting Files

- Logs: `normal_traffic.pcap`, `nmap_scan.pcap`
- Screenshots: `nmap_scan_commands_1.png`, `nmap_scan_commands_2.png`, `wireshark_filters_1.png`, `wireshark_filters_2.png`
- GRC File: [GRC_report.md](./GRC_report.md)
- README: [README.md](./README.md)

---

**End of Report**
