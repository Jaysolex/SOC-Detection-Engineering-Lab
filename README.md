# SOC-Detection-Engineering-Lab
End-to-end SOC detection engineering lab correlating network (Zeek, Suricata) and endpoint (Sysmon, Windows Event Logs) telemetry in Splunk to detect reconnaissance and PowerShell-based defense evasion mapped to MITRE ATT&amp;CK.

🛡️ SOC Detection Engineering Lab

Author: Solomon James
Role Target: SOC Analyst / Detection Engineer
Focus: Network + Endpoint Telemetry Correlation in Splunk
Frameworks: MITRE ATT&CK, Detection Engineering Lifecycle

📌 Overview

This repository documents an end-to-end SOC Detection Engineering lab designed to simulate how modern blue teams detect adversary behavior by correlating network telemetry (Zeek, Suricata) with endpoint telemetry (Sysmon, Windows Event Logs) inside Splunk.

Rather than focusing on alerts alone, this lab emphasizes:

Signal generation

Log fidelity

Detection logic

Cross-sensor correlation

ATT&CK-mapped detections

This project reflects real-world SOC workflows, not CTF-style exercises.



🎯 Objectives

Build a realistic SOC lab with multiple telemetry sources

Detect reconnaissance and defense evasion techniques

Correlate endpoint + network data in Splunk

Map detections to MITRE ATT&CK

Produce resume- and interview-ready evidence


🏗️ Architecture

Core Components:

Attack Host: Kali Linux

Endpoint: Windows 10 (Sysmon + Windows Security Logs)

Network Sensor: Ubuntu Server (Zeek + Suricata)

SIEM: Splunk Enterprise

Telemetry Flow:

Kali / Windows Activity
        ↓
Zeek / Suricata / Sysmon
        ↓
Splunk Indexers
        ↓
Detection Logic & Correlation

A full architecture diagram is included in /architecture/.


🧰 Tools & Technologies
Network Telemetry

Zeek

conn.log

dns.log

http.log

Suricata

flow events

alert events

Endpoint Telemetry

Sysmon

Event ID 1 (Process Creation)

Windows Security Logs

Event ID 4688

SIEM & Analysis

Splunk Enterprise

SPL (Search Processing Language)


🧪 Detection Scenarios
1️⃣ Network Reconnaissance

Techniques Detected:

TCP SYN scans

Service enumeration

Port scanning behavior

Telemetry Used:

Zeek conn.log

Suricata flow events

Detection Strategy:

High connection fan-out

Short-lived TCP sessions

Unusual destination port patterns

MITRE Mapping:

T1046 – Network Service Scanning


2️⃣ PowerShell Defense Evasion

Attack Simulation:
powershell.exe -nop -w hidden -c "Get-Process | Out-File C:\Users\Public\ps_test.txt"

Why This Matters:

-nop bypasses profiles

-w hidden suppresses user visibility

Common LOLBin abuse technique

Telemetry Used:

Sysmon Event ID 1

Windows Security Event ID 4688

Detection Strategy:

Command-line inspection

Parent/child process analysis

Hidden window execution flags

MITRE Mapping:

T1059.001 – PowerShell

TA0005 – Defense Evasion


🔍 Example Detection Logic (SPL)

index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1
| search Image="*powershell.exe*" CommandLine="*-nop*"
| table _time host User Image CommandLine ParentImage


🔗 Cross-Sensor Correlation

One of the key goals of this lab is proving correlation, not isolated alerts.

Examples include:

Same source IP observed in Zeek + Suricata

Endpoint execution linked to prior network activity

Timeline reconstruction across sensors

This mirrors how enterprise SOCs reduce false positives and improve confidence.


📁 Repository Structure

soc-detection-engineering-lab/
├── README.md
├── architecture/
│   └── soc_architecture_diagram.png
├── detections/
│   ├── network_recon.md
│   └── powershell_defense_evasion.md
├── screenshots/
│   ├── splunk_searches/
│   └── attack_execution/
└── notes/

