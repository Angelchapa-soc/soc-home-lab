# Phase 1 — Lab Build

## Requirements
- Host with **16 GB RAM** minimum (32 GB recommended), ~250 GB free disk
- Hypervisor: VirtualBox or VMware Workstation Player (free)
- ISOs: Windows Server 2022 and Windows 10/11 evaluation (Microsoft Evaluation Center), Ubuntu Server, Kali Linux

## Network
One **host-only / internal network** (e.g. `10.0.0.0/24`) so attack traffic never leaves the lab.

| VM | IP | RAM |
|---|---|---|
| DC01 | 10.0.0.10 | 4 GB |
| WS01 | 10.0.0.20 | 4 GB |
| SIEM (Ubuntu + Splunk) | 10.0.0.30 | 6–8 GB |
| ATTACK (Kali) | 10.0.0.50 | 2–4 GB |

## Steps
1. **Domain controller** — install Windows Server, promote to DC (`lab.local`), create test users and a few groups.
2. **Workstation** — install Windows 10/11 and join it to `lab.local`.
3. **Sysmon** — install on DC01 and WS01 with a community config (SwiftOnSecurity or Olaf Hartong's sysmon-modular):
   `sysmon64.exe -accepteula -i sysmonconfig.xml`
4. **Audit policy** — enable logon, account management, process creation (with command line) and service installation auditing via Group Policy.
5. **Splunk** — install Splunk Enterprise (free license) on the SIEM VM, enable receiving on port 9997, create indexes `wineventlog` and `sysmon`.
6. **Forwarders** — install the Splunk Universal Forwarder on DC01/WS01 and collect Security, System and Sysmon/Operational logs.
7. **Verify** — search `index=sysmon EventCode=1` and `index=wineventlog EventCode=4624` and confirm events arrive.
8. **Attack tooling** — install Atomic Red Team on WS01 (PowerShell `Invoke-AtomicRedTeam`) and take a VM snapshot before each test.

## Phase 1 checklist
- [ ] Domain working, WS01 joined
- [ ] Sysmon running on both Windows hosts
- [ ] Logs visible in Splunk
- [ ] Clean snapshots taken
