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
- [x] **Phase 4 – Automate:** [`alert_report.py`](scripts/alert_report.py) builds a daily Excel alert report (by severity, technique and host) and emails it
- [ ] **Phase 5 – OT bonus:** OpenPLC + Modbus traffic and detection of unauthorized writes

Setup guide: [docs/01-lab-setup.md](docs/01-lab-setup.md)

## 🔍 Detections
Each technique has a **Sigma** rule plus **Splunk SPL** and **CrowdStrike LogScale (LQL)** versions. Rules are drafts until validated against lab telemetry.

| ATT&CK ID | Technique | Simulation | Telemetry | Sigma | Write-up |
|---|---|---|---|---|---|
| T1110 | Brute Force | Failed logons against WS01 | EID 4625 | [rule](detections/sigma/t1110_failed_logons.yml) | _pending_ |
| T1059.001 | PowerShell | Encoded PowerShell command | Sysmon EID 1 | [rule](detections/sigma/t1059_001_encoded_powershell.yml) | _pending_ |
| T1053.005 | Scheduled Task | `schtasks /create` persistence | Sysmon EID 1 / EID 4698 | [rule](detections/sigma/t1053_005_scheduled_task.yml) | _pending_ |
| T1136.001 | Create Local Account | `net user /add` | EID 4720 | [rule](detections/sigma/t1136_001_local_account_created.yml) | _pending_ |
| T1003.001 | LSASS Memory | Access to lsass.exe | Sysmon EID 10 | [rule](detections/sigma/t1003_001_lsass_access.yml) | _pending_ |
| T1543.003 | Windows Service | New service installed | EID 7045 | [rule](detections/sigma/t1543_003_new_service.yml) | _pending_ |

All SPL searches: [detections/spl/windows_detections.spl](detections/spl/windows_detections.spl) · All LQL queries: [detections/lql/falcon_detections.lql](detections/lql/falcon_detections.lql)

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
