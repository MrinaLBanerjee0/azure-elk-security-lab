# Azure ELK Security Lab

This was my first larger SOC lab. I built it in Microsoft Azure and used Elastic Security to collect Windows and Linux telemetry, create detections, investigate controlled activity, and connect basic alerting to osTicket.

[Investigation report](SOC-INVESTIGATION-REPORT.md) · [Evidence](evidence/) · [Exported artifacts](artifacts/) · [Architecture diagram](diagrams/Diagram.svg)

## What I built

| Area | What I worked on |
| --- | --- |
| Cloud | Six Azure VMs across three regional VNets with two documented hub-to-spoke peerings |
| Telemetry | Windows Security, Sysmon, Defender, Linux system, and SSH events in Elastic |
| Detection | Custom SSH and Windows failed-logon threshold rules |
| Investigation | Alert review, raw-event analysis, source/user pivots, and disposition |
| Automation | Basic Elastic webhook/API actions that created osTicket tickets |
| Monitoring | Kibana views for SSH failures over time and top SSH source IPs |
| Controlled testing | SSH and RDP testing; Mythic HTTP profile and Apollo payload configuration |

## Architecture

![Azure ELK Security Lab architecture](diagrams/Diagram.png)

The diagram shows the Azure networking, endpoint telemetry, Fleet management, controlled test activity, alert-to-ticket flow, and the Mythic path I configured but did not successfully validate with a callback.

## Main workflow

```text
Controlled activity → endpoint telemetry → Elasticsearch
        → detection → alert → investigation → osTicket ticket
        → dashboard review → tuning findings
```

### SSH test

- I generated controlled failed SSH logons from myVm (`10.1.0.5`) to Ubuntu (`10.1.0.4`).
- Elastic Agent collected the Linux authentication events.
- The rule threshold was `5`, grouped by `user.name` and `source.ip`, running every `5m` with a `10m` lookback.
- I reviewed the underlying events and pivoted on the user and source IP.
- The webhook action created a basic osTicket ticket.

### Windows / RDP test

- Kali was used for authorized reconnaissance and unsuccessful RDP authentication testing against the Windows endpoint.
- Windows failed-logon events reached Elastic during the test.
- The historical rule was named `win Rdp brute force`, but its exported query was only `event.code: 4625 and agent.name: win and user.name: Mrinal`.
- The threshold was `2`, grouped by `source.ip` and `user.name`, running every `30s` with a `330s` lookback.
- Because the query did not check Logon Type `10`, I do not present this as a proven RDP-specific detection. It was a general Windows failed-logon rule used during an RDP-related test.
- I did not confirm a valid credential or successful RDP session.

## Technology used

| Layer | Technology |
| --- | --- |
| Cloud and networking | Microsoft Azure, VNets, subnets, VNet peering |
| Collection and management | Elastic Agent, Fleet Server |
| SIEM / analytics | Elasticsearch, Kibana, Elastic Security |
| Windows telemetry | Security/Application/System logs, Sysmon, Microsoft Defender, Elastic Defend |
| Linux telemetry | System and SSH authentication logs |
| Controlled testing | Kali Linux, Nmap, Crowbar, Mythic, Apollo |
| Case management | osTicket, webhook/API connector |

## What I verified

- I captured healthy Fleet enrollment for the Windows, Ubuntu, and Fleet hosts.
- The Windows applied-policy snapshot includes Security/Application/System collection, `win-sysmon`, `win-defender`, and Elastic Defend with the `EDRComplete` preset.
- The exported Defender input contains Event IDs `1116`, `1117`, and `5001`. It does not contain `5007`; a separate Kibana screenshot shows that Event ID `5007` was still observed in collected data.
- The Ubuntu applied policy reads `/var/log/auth.log*`, `/var/log/secure*`, `/var/log/messages*`, `/var/log/syslog*`, and `/var/log/system*` into `system.auth` and `system.syslog`.
- Both custom validation rules executed and generated alerts.
- The Elastic-to-osTicket connector test succeeded, and the API-created tickets were captured.
- The dashboard export contains two `logs-*` panels: SSH failures over time and top SSH source IPs.
- I configured the Mythic HTTP profile and created an Apollo payload, but I did **not** prove an active Apollo callback or session.

## Exported artifacts

The [`artifacts/`](artifacts/) folder contains the Kibana dashboard export, a sanitized rules/connector export, Windows and Ubuntu Fleet-policy snapshots, and the Sysmon configuration used in the lab.

These are historical lab artifacts, not production-ready templates. Some values are intentionally left as they existed in the lab, including `agent.name: ubantu`, `agent.name: win`, and `user.name: Mrinal`. The thresholds and schedules would need retuning for another environment. The connector keeps the private lab URL but the API key was replaced with `REDACTED`.

## Limitations

This was a learning environment, not a production SOC. It does not demonstrate 24/7 operations, production-scale tuning, a successful RDP compromise, a successful Mythic callback, full SOAR orchestration, or mature case-management metrics.

The osTicket action body was basic: essentially `Investigate Rule: <rule name>`. One captured view also showed 440 alerts from the historically named Windows rule out of 444 medium-severity alerts, which is a clear sign that the rule needed more suppression and tuning.

## Repository guide

| Section | Contents |
| --- | --- |
| [Environment](environment/) | VM inventory, VNets, subnets, regions, and peerings |
| [Telemetry](telemetry/) | Windows/Linux collection and Fleet details |
| [Attack simulation](attack-simulation/) | Controlled RDP testing and Mythic configuration |
| [Detections](detections/) | SSH and Windows failed-logon rule behavior |
| [Investigations](investigations/) | Event review and analyst pivots |
| [Automation](automation/) | Basic Elastic-to-osTicket ticket creation |
| [Dashboards](dashboards/) | SSH monitoring visualizations |
| [Artifacts](artifacts/) | Exports, applied-policy snapshots, and Sysmon configuration |
| [Evidence](evidence/) | Screenshots and claim-to-proof mapping |
| [Full report](SOC-INVESTIGATION-REPORT.md) | Timeline, findings, limitations, and lessons learned |

## Security note

The rules export has one intentional redaction: the original osTicket API-key value was replaced with `REDACTED`. I did not silently change the detection logic while sanitizing the file.
