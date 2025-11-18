# Network Scanning & Traffic Analysis Lab

**SOC-style exercise** demonstrating detection and analysis of network reconnaissance activity using Nmap and Wireshark.

- **Author:** Gene Crittenden  
- **Date:** 13 November 2025  
- **Web Server VM:** 192.168.56.101  
- **Client VM:** 192.168.56.102  
- **Environment:** VirtualBox Host-Only Network (Isolated SOC Lab)  

---

## Overview

This lab demonstrates how a SOC analyst can detect and analyze **network reconnaissance** activity within a controlled environment.  

The exercise simulates an adversary performing network scans to identify live hosts and open ports. Packet captures are analyzed with **Wireshark** to observe scan patterns and packet structures.  

**Objectives:**

1. Detect live hosts and open ports using Nmap.  
2. Capture reconnaissance activity with Wireshark.  
3. Analyze packet traffic to identify scan patterns.  
4. Document findings in a SOC-style report.  
5. Map observed activity to GRC frameworks for compliance evidence.  

**Why This Lab Matters:**  
Understanding how reconnaissance traffic appears on the network is essential for SOC monitoring, incident detection, and compliance reporting.

---

## Lab Setup

| VM | Role | IP Address | Tools Installed |
|----|------|------------|----------------|
| `web-server` | Target | `192.168.56.101` | nginx, Wireshark |
| `client` | Scanner | `192.168.56.102` | Nmap, curl, Wireshark |

- **Network:** Host-only network (isolated, no internet)  

---

## Step 1 — Baseline Traffic Capture

1. Start **Wireshark** on the `web-server` VM and capture traffic on the host-only adapter.  
2. Generate normal web traffic:
```bash
curl http://192.168.56.101
ping -c 4 192.168.56.101
```

3. Stop the capture and export for analysis.

---

## Step 2 — Network Reconnaissance & Port Scanning

1. Start Wireshark on the web-server VM.
2. From the client VM, discover live hosts:
```bash
nmap -sn 192.168.56.1/24
```
3. Perform a full TCP connect scan on the web server:
```bash
nmap -sT -p 1-1024 192.168.56.101
```
4. Perform a service/version scan:
```bash
nmap -sV 192.168.56.101
```
5. Run additional suspicious queries:
```
curl http://192.168.56.101/admin
curl http://192.168.56.101/../../etc/passwd
```
6. Stop the capture and export for analysis.

---

## Step 3 — Packet Capture Analysis

Analyze the capture from Step 2 to identify reconnaissance patterns.

<details> <summary>Wireshark Filters</summary>

```wireshark
TCP SYN packets (scan behavior): tcp.flags.syn == 1 && tcp.flags.ack == 0
Source IP filter: ip.src == 192.168.56.102
General communication: ip.addr == 192.168.56.101 || ip.addr == 192.168.56.102
RST responses (closed ports)
SYN-ACK responses (open ports)
```
</details>

--- 

## Step 4. SOC Response

Upon detecting the reconnaissance activity from the client VM, a SOC analyst would take the following steps:
<details><summary>SOC Response</summary>
1. **Confirm and Validate Findings**
   - Review packet captures and Nmap scan logs to confirm the source and type of activity.  
   - Identify the targeted hosts and scanned ports.  

2. **Assess Risk and Impact**
   - Determine whether the reconnaissance indicates a potential threat or just benign testing.  
   - Assess which services and ports are exposed and whether sensitive systems are affected.  

3. **Containment and Mitigation**
   - If the scanning is unauthorized, implement temporary firewall rules or access restrictions to block the scanning source.  
   - Review firewall configurations and access controls to ensure critical services are protected.  

6. **Monitoring and Alerting**
   - Set up or refine IDS/IPS rules to detect similar scans in the future.  
   - Create alerts for repeated reconnaissance attempts or suspicious traffic patterns.  

7. **Documentation and Reporting**
   - Document the detected activity, analysis steps, and mitigation actions in an **Incident Report**.  
   - Include supporting evidence such as packet captures, Nmap logs, and screenshots.  
   - Map findings to GRC frameworks to support compliance and auditing requirements.  
</details>

**Outcome:**  
The SOC is able to detect and analyze network reconnaissance, confirm the impact on critical assets, and implement both immediate and long-term measures to reduce exposure. Documentation provides evidence for compliance and continuous improvement of monitoring processes.

---

## Step 6. GRC Mapping
| Framework       | Control                             | Purpose                                              |
| --------------- | ----------------------------------- | ---------------------------------------------------- |
| NIST SP 800-53  | SI-4: Information System Monitoring | Monitor and detect reconnaissance activity           |
| ISO 27001:2022  | A.12.4: Logging and Monitoring      | Log and review network security events               |
| CIS Controls v8 | 8.1: Audit Logging                  | Capture and analyze scanning behavior for compliance |

---

## Skills Demonstrated
- Nmap scanning & host discovery
- Network traffic analysis with Wireshark
- Detection of reconnaissance and port scanning activity
- SOC-style workflow: Detect → Analyze → Document
- Linking technical observations to GRC frameworks

---

## Lessons Learned
- Reconnaissance traffic produces recognizable patterns in packet captures.
- Wireshark enables detailed visibility into TCP/IP scan behavior.
- TCP SYN, SYN-ACK, and RST analysis aids in distinguishing allowed vs. blocked services.
- Proper logging and monitoring are critical to early detection of network scanning.
- SOC observations can be tied directly to compliance and GRC objectives.

---

## Supporting Files
**Reports**
- [Incident Report](./incident_report.md)
- [GRC Report](./grc_report.md)

[Logs](./logs)
- Nmap Scans & Wrieshark Packet Captures
  - _The original .pcapng file is kept offline and available upon request._

[Screenshots](./screenshots)
- Nmap Scans & Wireshark Filters

---

**End of Report**
