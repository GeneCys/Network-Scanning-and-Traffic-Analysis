# Network-Scanning-and-Traffic-Analysis
Exercise in detecting and analysing network reconnaissance 

- **Author:** Gene Crittenden
- **Date:** 2025-11-13
- **Environment:** VirtualBox Host-Only Network (Isolated SOC Lab)
- **Tools Used:** Nmap, Wireshark, nginx, curl

---

Overview

This exercise demonstrates how a SOC analyst can detect and analyze **network reconnaissance** activity using **Nmap** and **Wireshark** within an isolated lab environment.

The activity simulates a potential adversary performing network scans to identify live hosts and open ports.
Wireshark captures were analyzed to observe the patterns and packet structures associated with these scans.

---

Lab Setup

| VM | Role | IP Address | Tools Installed |
|----|------|-------------|-----------------|
| `web-server` | Target | 192.168.56.101 | nginx, Wireshark |
| `client` | Scanner | 192.168.56.102 | Nmap, curl, Wireshark |

**Network:** Host-only network (no internet access)

---

Exercise Steps

**Step 1 - Baseline Capture**
1. Start Wireshark on the `web-server` VM and begin capturing traffic on the host-only adapter.
2. Generate normal web traffic:
- curl http://192.168.56.101
- ping -c 4 192.168.56.101
3. Stop the capture and export it.

**Step 2 - Network Reconnaissance & Port Scanning**
1. Start Wireshark on the `web-server` VM and begin capturing traffic on the host-only adapter.
2. From the `client` VM run Nmap to discover live hosts on the host-only network:
- nmap -sn 192.168.56.1/24
3. From the `client` VM run Nmap to perform a full TCP connect scan against the web server:
- nmap -sT -p 1-1024 192.168.56.101
4. From the `client` VM run Nmap to perform a service version scan of the web server.
- nmap -sV 192.168.56.101
5. OPTIONAL: Run further suspicious commands against the web server.
- curl http://192.168.56.101/admin
- curl http://192.168.56.101/../../etc/passwd
6. Stop the capture and export it.

**Step 3 - Capture Analysis**
1. Using the capture from Step 2, analyse the packets with Wireshark. Look For:
- TCP SYN packets in rapid succession
- RST responses from closed ports
- SYN-ACK responses on port 80
2. Refine analysis by using filters:
- tcp.flags.syn == 1 && tcp.flags.ack == 0
- ip.src == 192.168.56.102

**SOC Learning Objectives**
- Understand how reconnaissance and port scan appear in raw network captures.
- Identify TCP SYN, SYN-ACK, and RST packet patterns.
- Learn how to correlate network activity with potential threat behavior
- Practice documenting findings in a SOC-style report

**GRC Learning Objectives**
| Framework | Control | Purpose | 
|----|------|-------------|
| NIST SP 800-53 | SI-4: Information System Monitoring | Monitoring and detection of reconnaissance activity |
| ISO 27001:2022 | A.12.4: Logging and Monitoring | Logging and review of network security events |
| CIS COntrols v8 | 8.1: Audit Logging | Capture and analysis of scanning behaviour for compliance |

**Supporting Files**
- incident_report.md
- grc_report.md

Logs
- normal_traffic.pcap
- nmap_scan.pcap

Screenshots
- wireshark_filters_1.png
- wireshark_filters_2.png
- nmap_scan_commands_1.png
- nmap_scan_commands_2.png

**Lessons Learned**
- Nmap scanning produces distinct, recognizable network patterns.
- Wireshark provides clear visibility into reconnaissance activity.
- Proper loggin and monitoring can detect port scanning early.
- Linking SOC observations to GRC frameworks reinforces complaince and reporting practices.
