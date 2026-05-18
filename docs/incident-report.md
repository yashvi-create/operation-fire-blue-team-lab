# 🛡️ Operation Fire — Incident Report
**Lab:** Blue Team Endpoint Monitoring & Investigation  
**Analyst:** Yashvi Thakar  
**Date:** May 2026  
**Status:** ✅ Completed  

---

## 📋 Executive Summary

This report documents the findings from Operation Fire — a hands-on Blue Team security lab simulating real-world endpoint monitoring and threat detection. The lab environment consisted of a Kali Linux attacker VM and a Windows target endpoint monitored using Sysmon, Windows Event Viewer, and Wireshark.

A total of **4 threat scenarios** were simulated, detected, and documented.

---

## 🔴 Finding 1 — Encoded PowerShell Command Detected

**Severity:** High  
**Tool Used:** Windows Event Viewer + PowerShell  
**Event:** Attacker executed a base64 encoded PowerShell command using `powershell -enc`

**What Happened:**
- A base64 encoded command was executed via PowerShell
- This is a common obfuscation technique used by attackers to hide malicious scripts
- The command was flagged and identified during endpoint monitoring

**Detection Method:** PowerShell command history + Sysmon Process Create event

**Recommendation:** Block encoded PowerShell execution via Group Policy. Enable ScriptBlock logging.

---

## 🔴 Finding 2 — Rogue Account Creation (Privilege Escalation)

**Severity:** High  
**Tool Used:** PowerShell + Windows Event Viewer  
**Event:** Unauthorised user account `hacker_test` created via `net user hacker_test /add`

**What Happened:**
- A rogue user account was created on the Windows endpoint
- This simulates a real privilege escalation attack
- The command completed successfully — meaning no preventive control blocked it

**Detection Method:** Windows Security Event Log — Account Creation Event

**Recommendation:** Restrict `net user` command to administrators only. Alert on all new account creation events.

---

## 🟠 Finding 3 — Sysmon Captured 604 Process Events

**Severity:** Medium  
**Tool Used:** Sysmon + Windows Event Viewer  
**Event:** 604 operational events captured including Process Create (ID 1) and Process Terminate (ID 5)

**What Happened:**
- Sysmon logged all process activity on the endpoint in real time
- Event ID 1 (Process Create) and Event ID 5 (Process Terminate) were the most frequent
- Anomalous processes were identified and cross-referenced with known attack patterns

**Detection Method:** Sysmon Operational Log analysis

**Recommendation:** Set up automated alerting for suspicious process names (cmd.exe, powershell.exe spawned from Office apps etc.)

---

## 🟠 Finding 4 — Network Traffic Anomalies via Wireshark

**Severity:** Medium  
**Tool Used:** Wireshark  
**Event:** TCP retransmissions, TLSv1.2 application data and UDP streams captured

**What Happened:**
- Live network traffic was captured during the attack simulation
- TCP retransmissions detected — possible signs of network instability or evasion
- TLSv1.2 encrypted traffic observed on port 443
- UDP streams to port 443 identified — unusual and worth investigating

**Detection Method:** Wireshark packet capture and protocol analysis

**Recommendation:** Monitor for unusual UDP traffic on port 443. Investigate TLS certificates for suspicious domains.

---

## ✅ Summary Table

| # | Finding | Severity | Detected | Tool |
|---|---|---|---|---|
| 1 | Encoded PowerShell execution | 🔴 High | ✅ Yes | Sysmon + PowerShell |
| 2 | Rogue account creation | 🔴 High | ✅ Yes | Event Viewer |
| 3 | 604 process events logged | 🟠 Medium | ✅ Yes | Sysmon |
| 4 | Network traffic anomalies | 🟠 Medium | ✅ Yes | Wireshark |

---

## 🎯 Conclusion

All 4 threat scenarios were successfully **simulated, detected, and documented**. This lab demonstrates practical Blue Team skills including endpoint monitoring, log analysis, network traffic investigation and incident documentation — core competencies for SOC analyst roles.

---

*Report by Yashvi Thakar — github.com/yashvi-create*
