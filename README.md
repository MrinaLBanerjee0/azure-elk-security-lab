# Azure ELK Security Lab

An evidence-backed SOC lab built in Microsoft Azure to demonstrate the analyst workflow from controlled security activity to centralized telemetry, custom detections, investigation, basic automated ticketing, and dashboard monitoring.

[Read the full investigation report](SOC-INVESTIGATION-REPORT.md) · [Review the evidence index](evidence/) · [Inspect reusable artifacts](artifacts/) · [Open the editable architecture](diagrams/Diagram.svg)

## Recruiter Summary

| Area | Demonstrated outcome |
| --- | --- |
| Cloud architecture | Six Azure VMs across three regional VNets with two documented hub-to-spoke peerings |
| Telemetry | Windows Security, Sysmon, Defender, Linux system, and SSH events centralized in Elastic |
| Detection | Custom SSH and Windows failed-logon threshold rules executed and generated alerts |
| Investigation | Alert review, raw-event analysis, source/user pivots, and evidence-based disposition |
| Automation | Elastic webhook/API actions created basic osTicket tickets for both custom rules |
| Monitoring | Kibana visualizations tracked SSH failures over time and top SSH source IPs |
| Adversary simulation | Controlled SSH and RDP testing; Mythic HTTP profile and Apollo payload configured |

## Architecture

![Azure ELK Security Lab architecture](diagrams/Diagram.png)

The diagram distinguishes Azure VNet peering, endpoint telemetry, Fleet management, controlled test activity, alert-to-ticket automation, and the unconfirmed Mythic callback path.

## Validated SOC Workflow

```text
Controlled Activity → Endpoint Telemetry → Elasticsearch
        → Detection → Alert → Investigation → osTicket Ticket
        → Dashboard Review → Tuning Findings
```

### SSH Use Case

- Controlled failed SSH logons originated from myVm (`10.1.0.5`) and targeted Ubuntu (`10.1.0.4`).
- Linux authentication events were collected through Elastic Agent.
- The rule threshold was `5`, grouped by `user.name` and `source.ip`, running every `5m` with a `10m` lookback.
- The alert was investigated using underlying events and user/source pivots.
- The webhook action created a basic osTicket ticket.

### Windows / RDP Test Context

- Kali performed authorized reconnaissance and unsuccessful RDP authentication testing against the Windows endpoint.
- Windows failed-logon telemetry reached Elastic during the controlled test.
- The historical rule name was `win Rdp brute force`, but its exported query was only `event.code: 4625 and agent.name: win and user.name: Mrinal`.
- The rule threshold was `2`, grouped by `source.ip` and `user.name`, running every `30s` with a `330s` lookback.
- Because the query did not validate Logon Type `10`, the rule itself was a general Windows failed-logon rule—not a proven RDP-specific detection.
- No valid credential or successful RDP session was confirmed.

## Technology Stack

| Layer | Technology |
| --- | --- |
| Cloud and networking | Microsoft Azure, VNets, subnets, VNet peering |
| Collection and management | Elastic Agent, Fleet Server |
| Data and analytics | Elasticsearch, Kibana, Elastic Security |
| Windows telemetry | Security/Application/System logs, Sysmon, Microsoft Defender, Elastic Defend |
| Linux telemetry | System and SSH authentication logs |
| Controlled testing | Kali Linux, Nmap, Crowbar, Mythic, Apollo |
| Case management | osTicket, webhook/API connector |

## Evidence-Backed Results

- Healthy Fleet enrollment was captured for the Windows, Ubuntu, and Fleet hosts.
- The Windows applied-policy snapshot includes Security/Application/System collection, `win-sysmon`, `win-defender`, and Elastic Defend with the `EDRComplete` preset.
- The exported Defender input configures Event IDs `1116`, `1117`, and `5001`. It does not configure `5007`; a separate Kibana screenshot shows Event ID `5007` was observed in collected data.
- The Ubuntu applied policy reads `/var/log/auth.log*`, `/var/log/secure*`, `/var/log/messages*`, `/var/log/syslog*`, and `/var/log/system*` into `system.auth` and `system.syslog`.
- Both custom validation rules executed and generated alerts.
- The Elastic-to-osTicket connector test succeeded, and API-created tickets were captured.
- The dashboard export contains two `logs-*` panels: SSH failures over time and top SSH source IPs.
- Mythic profile and payload creation were completed, but an active Apollo callback was not proven.

## Reusable Artifacts

The [`artifacts/`](artifacts/) directory contains the real Kibana dashboard export, a credential-sanitized rules/connector export, Windows and Ubuntu applied Fleet-policy snapshots, and the third-party Sysmon configuration used in the lab. The API-key value is the only field changed in the sanitized NDJSON; the historical rule names, descriptions, queries, thresholds, schedules, grouping, and actions remain unchanged.

## Scope and Limitations

This is a controlled learning environment, not a production SOC. It does not demonstrate 24/7 operations, production-scale tuning, a successful RDP compromise, a successful Mythic callback, full SOAR orchestration, or case lifecycle metrics. The osTicket action body was basic—essentially `Investigate Rule: <rule name>`—and one captured view showed 440 alerts from the historically named Windows rule out of 444 medium-severity alerts, so suppression and tuning remain necessary.

## Repository Guide

| Section | Contents |
| --- | --- |
| [Environment](environment/) | VM inventory, VNets, subnets, regions, and peerings |
| [Telemetry](telemetry/) | Windows/Linux collection and Fleet control-plane boundaries |
| [Attack simulation](attack-simulation/) | Controlled RDP testing and Mythic configuration |
| [Detections](detections/) | Exact SSH and Windows failed-logon rule behavior |
| [Investigations](investigations/) | Event review and analyst pivots |
| [Automation](automation/) | Basic Elastic-to-osTicket ticket creation |
| [Dashboards](dashboards/) | SSH monitoring visualizations |
| [Artifacts](artifacts/) | Reusable exports, applied-policy snapshots, and Sysmon configuration |
| [Evidence](evidence/) | Claim-to-evidence map with proof boundaries |
| [Full report](SOC-INVESTIGATION-REPORT.md) | End-to-end findings, timelines, limitations, and lessons learned |

## Security and Evidence Handling

The rules export contains one redaction: the original osTicket API-key value was replaced with `REDACTED`. No detection logic was silently improved. Screenshots and configuration files are described only to the limit of what they directly prove.
