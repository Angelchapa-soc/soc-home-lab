# SOC Home Lab — Attack Simulation & Detection Engineering

A hands-on lab where I simulate real attacker techniques on a small Windows domain, collect the telemetry in a SIEM, and write detections mapped to **MITRE ATT&CK**.

> Status: 🚧 in progress — sections are filled in as each phase is completed.

## 🎯 Goals
- Build a realistic Windows + Active Directory environment with full endpoint logging
- Simulate attacker techniques with **Atomic Red Team**
- Detect each technique in **Splunk** (SPL), document the investigation, and tune false positives
- Keep a reusable detection library in **Sigma**, with SPL and CrowdStrike LogScale (LQL) versions

## 🏗️ Architecture
| VM | Role | Software |
|---|---|---|
| DC01 | Domain Controller | Windows Server, Active Directory, Sysmon |
| WS01 | Victim workstation | Windows 10/11, Sysmon, Splunk Universal Forwarder |
| SIEM | Log collection & analysis | Splunk Enterprise (free license) or Wazuh |
| ATTACK | Attacker machine | Kali Linux, Atomic Red Team |

Network: isolated host-only network (no internet exposure).

## 🗺️ Roadmap
- [ ] **Phase 1 – Build:** VMs, domain, Sysmon (SwiftOnSecurity / Olaf config), log forwarding to the SIEM
- [ ] **Phase 2 – Baseline:** document normal activity and key Windows Event IDs (4624, 4625, 4688, 4720, 7045…)
- [ ] **Phase 3 – Attack & Detect:** one write-up per technique (see table below)
- [ ] **Phase 4 – Automate:** Python script for alert notifications and a daily summary report
- [ ] **Phase 5 – OT bonus:** OpenPLC + Modbus traffic and detection of unauthorized writes

## 🔍 Detections
| ATT&CK ID | Technique | Simulation | Detection | Write-up |
|---|---|---|---|---|
| T1110 | Brute Force | Failed logons against WS01 | Many 4625 events from one source | _pending_ |
| T1059.001 | PowerShell | Encoded PowerShell command | Sysmon EID 1 with `-enc` | _pending_ |
| T1053.005 | Scheduled Task | `schtasks /create` persistence | EID 4698 / Sysmon EID 1 | _pending_ |
| T1136.001 | Create Local Account | `net user /add` | EID 4720 | _pending_ |
| T1003.001 | LSASS Memory | Access to lsass.exe | Sysmon EID 10 | _pending_ |
| T1543.003 | Windows Service | New service installed | EID 7045 | _pending_ |

## 📁 Repository layout
```
docs/          setup guides for each phase
detections/
  sigma/       vendor-neutral rules
  spl/         Splunk searches
  lql/         CrowdStrike LogScale queries
writeups/      one investigation report per technique
scripts/       Python automation
```

## 🧰 Tools
Splunk · Sysmon · Atomic Red Team · Windows Server / AD · Kali Linux · Python · MITRE ATT&CK Navigator
