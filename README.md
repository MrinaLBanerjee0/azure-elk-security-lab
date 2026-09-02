# Azure ELK Security Lab

An evidence-backed SOC lab built in Microsoft Azure to demonstrate the complete analyst workflow from controlled security activity to centralized telemetry, custom detections, investigation, automated ticketing, and dashboard monitoring.

[Read the full investigation report](SOC-INVESTIGATION-REPORT.md) · [Review the evidence index](evidence/) · [Open the editable architecture](diagrams/Diagram.svg)

## Recruiter Summary

| Area | Demonstrated outcome |
| --- | --- |
| Cloud architecture | Six Azure VMs across three regional VNets with two documented hub-to-spoke peerings |
| Telemetry | Windows Security, Sysmon, Defender, Linux system, and SSH events centralized in Elastic |
| Detection | Custom SSH and Windows failed-logon threshold rules executed and generated alerts |
| Investigation | Alert review, raw-event analysis, source/user pivots, and evidence-based disposition |
| Automation | Elastic webhook/API integration created SSH and RDP incidents in osTicket |
| Monitoring | Kibana visualizations tracked SSH failures over time and top source IPs |
| Adversary simulation | Controlled SSH and RDP testing; Mythic HTTP profile and Apollo payload configured |

## Architecture

![Azure ELK Security Lab architecture](diagrams/Diagram.png)

The diagram distinguishes five different relationships: Azure VNet peering, endpoint telemetry, Fleet management, controlled test activity, and alert-to-ticket automation. Configured paths with unconfirmed outcomes are shown separately from verified activity.

## Validated SOC Workflow

```text
Controlled Activity → Endpoint Telemetry → Elasticsearch
        → Detection → Alert → Investigation → osTicket Incident
        → Dashboard Review → Tuning Findings
```

### SSH Use Case

- Controlled failed SSH logons originated from myVm (`10.1.0.5`) and targeted Ubuntu (`10.1.0.4`).
- Linux authentication events were collected through Elastic Agent.
- The lab rule triggered at five failed events grouped by source IP and username.
- The alert was investigated using the underlying events and user/source pivots.
- The workflow generated an osTicket incident.

### Windows / RDP Use Case

- Kali performed authorized reconnaissance and failed RDP authentication testing against the Windows endpoint.
- Windows authentication telemetry reached Elastic and generated alerts during the test.
- The captured lab rule used Windows Event ID `4625` with a threshold of two events grouped by source IP and username.
- No valid credential or successful RDP session was confirmed.
- Event ID `4625` covers failed Windows logons generally; a production RDP-specific rule should also validate Logon Type `10`.

## Technology Stack

| Layer | Technology |
| --- | --- |
| Cloud and networking | Microsoft Azure, VNets, subnets, VNet peering |
| Collection and management | Elastic Agent, Fleet Server |
| Data and analytics | Elasticsearch, Kibana, Elastic Security |
| Windows telemetry | Security events, Sysmon, Microsoft Defender |
| Linux telemetry | System and SSH authentication logs |
| Controlled testing | Kali Linux, Nmap, Crowbar, Mythic, Apollo |
| Case management | osTicket, webhook/API connector |

## Evidence-Backed Results

- Healthy Fleet enrollment was captured for the Windows, Ubuntu, and Fleet hosts.
- Sysmon Event ID `1`, Defender Event ID `5007`, and Ubuntu SSH events were searchable in Elastic.
- Both custom validation rules executed and generated alerts.
- The SSH investigation preserved event detail and user/source correlation views.
- The Elastic-to-osTicket connector test succeeded, and alert-derived tickets were created.
- The monitoring dashboard visualized SSH failures over time and top sources.
- The RDP authentication attempt did not find a valid credential.
- Mythic profile and payload creation were completed, but an active Apollo callback was not proven.

## Scope and Limitations

This is a controlled learning environment, not a production SOC. The evidence supports lab execution and analyst workflow, but it does not demonstrate 24/7 operations, production-scale tuning, a successful RDP compromise, or a successful Mythic callback. One captured view contained 440 RDP-rule alerts out of 444 medium-severity alerts, so threshold and suppression tuning remain necessary.

Screenshots prove observed lab state; they are not substitutes for reusable configuration exports. Sanitized rule, dashboard, Fleet, Sysmon, and connector artifacts remain the main reproducibility improvement.

## Repository Guide

| Section | Contents |
| --- | --- |
| [Environment](environment/) | VM inventory, VNets, subnets, regions, and peerings |
| [Telemetry](telemetry/) | Windows/Linux collection and corrected Fleet control-plane flow |
| [Attack simulation](attack-simulation/) | Controlled RDP testing and Mythic configuration |
| [Detections](detections/) | SSH and Windows failed-logon rule validation |
| [Investigations](investigations/) | Event review and analyst pivots |
| [Automation](automation/) | Elastic-to-osTicket incident creation |
| [Dashboards](dashboards/) | SSH monitoring visualizations |
| [Evidence](evidence/) | Claim-to-evidence map with proof boundaries |
| [Full report](SOC-INVESTIGATION-REPORT.md) | End-to-end findings, timelines, limitations, and lessons learned |

## Security and Evidence Handling

The public architecture intentionally omits public IP addresses, credentials, tokens, callback keys, and personal identifiers. Historical screenshots should be reviewed and opaquely redacted before reuse outside this portfolio. Every caption and conclusion is limited to what the captured evidence directly demonstrates.
