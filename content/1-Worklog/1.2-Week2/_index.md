---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Week 2 Objectives

* Understand AWS VPC networking concepts: VPC CIDR blocks, public/private subnets, Internet Gateways, Security Groups vs. Network ACLs.
* Set up a dedicated Windows Server / Windows 11 endpoint threat detection environment.
* Deploy Elastic Agent & Sysmon with custom SwiftOnSecurity configuration for full endpoint telemetry collection.
* Execute 7 Atomic Red Team attack simulations covering credential dumping, privilege escalation, persistence, and execution techniques.
* Engineer custom KQL detection rules and write 4 comprehensive threat hunt reports.

### Tasks Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study AWS VPC networking: public vs. private subnets, route tables, Security Groups, NACLs.<br>- Configure network isolation rules for attack simulation environment. | 06/22/2026 | 06/22/2026 | <https://000003.awsstudygroup.com><br><https://000092.awsstudygroup.com> |
| Tue | - Provision Windows 11 / Windows Server endpoint environment.<br>- Install Sysmon v15+ with SwiftOnSecurity configuration for process tracking.<br>- Install Elastic Agent and establish enrollment with Elastic SIEM. | 06/23/2026 | 06/23/2026 | <https://000093.awsstudygroup.com><br><https://github.com/SwiftOnSecurity/sysmon-config> |
| Wed | - Execute Endpoint Attack Scenarios 1–3:<br>&emsp;+ Windows Process Creation (T1059.001 - PowerShell Execution)<br>&emsp;+ Credential Dumping (T1003.001 - LSASS Memory Dumping via Mimikatz/ProcDump)<br>&emsp;+ Privilege Escalation (T1548.002 - Bypass UAC). | 06/24/2026 | 06/24/2026 | Atomic Red Team Library |
| Thu | - Execute Endpoint Attack Scenarios 4–7:<br>&emsp;+ Persistence via Registry Run Keys (T1547.001)<br>&emsp;+ Scheduled Task Creation (T1053.005)<br>&emsp;+ Defense Evasion (T1562.001 - Impair Defenses)<br>&emsp;+ Discovery & Enumeration (T1082 - System Information Discovery). | 06/25/2026 | 06/25/2026 | Atomic Red Team Library |
| Fri | - Verify telemetry ingestion in Elastic SIEM (`winlogbeat-*` & `logs-windows.*`).<br>- Write and validate 7 custom KQL detection rules in SIEM.<br>- Author 4 detailed threat hunt reports documenting attack vectors, logs, and mitigation recommendations. | 06/26/2026 | 06/26/2026 | Elastic Security Docs |

### Week 2 Achievements

* Mastered AWS VPC networking fundamentals, subnet routing, and firewall/security group configuration.
* Successfully built a fully instrumented Windows threat detection lab using Sysmon and Elastic Agent.
* Simulated 7 real-world endpoint attack scenarios using Atomic Red Team frameworks.
* Developed 7 high-fidelity KQL detection rules targeting process creation, credential access, and persistence.
* Authored 4 comprehensive threat hunt reports establishing an active detect-hunt-harden loop.
