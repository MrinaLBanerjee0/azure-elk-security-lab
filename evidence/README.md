# Evidence Index

This index maps each material project claim to the screenshot that supports it and states what the image does **not** prove. The evidence demonstrates observed lab activity; it is not a replacement for exported rules, configurations, or infrastructure-as-code.

[Return to project overview](../README.md) · [Read the full investigation report](../SOC-INVESTIGATION-REPORT.md)

## Infrastructure

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| Six Azure VMs were used | [VM inventory](azure/azure-vm-inventory.png) | Captured VM inventory and deployment context | Does not by itself prove service health |
| Hub-to-spoke peerings existed | [VNet peerings](azure/azure-vnet-peerings.png) | The two documented peerings involving `elaskiba-vnet` | No direct `ubuntu-vnet ↔ win-vnet` peering is claimed |
| Elaskiba used the documented private network | [Elaskiba network](azure/elaskiba-network.png) | Elaskiba network interface, private IP, subnet, and VNet context | Does not prove every route or NSG rule |
| myVm used the documented private network | [myVm network](azure/myvm-network.png) | myVm network interface, private IP, subnet, and VNet context | Does not prove direct connectivity to the Windows spoke |

## Telemetry and Fleet

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| Agents were enrolled and healthy | [Fleet agents](telemetry/fleet-agents.png) | Captured healthy status for `win`, `ubuntu`, and `fleet` | Health at capture time is not continuous availability |
| Windows integrations were configured | [Windows integrations](telemetry/windows-telemetry-integrations.png) | The Windows policy included Defender and Sysmon integrations | Configuration does not prove every dataset was populated |
| Sysmon process telemetry arrived | [Sysmon process create](telemetry/sysmon-process-create.png) | A Sysmon Event ID `1` process-create event was searchable | One event does not establish complete Sysmon coverage |
| Defender telemetry arrived | [Defender events](telemetry/windows-defender-events.png) | Defender Event ID `5007` records were searchable | Does not prove prevention or full endpoint coverage |
| Ubuntu SSH events arrived | [Ubuntu SSH events](telemetry/ubuntu-ssh-events.png) | SSH authentication activity was ingested from Ubuntu | Does not alone establish attack intent |

Elastic Agents send endpoint telemetry to Elasticsearch. Fleet Server manages enrollment, policy delivery, integrations, actions, status, and health; it is not shown as the telemetry relay or analysis engine.

## Controlled Attack Simulation

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| RDP service reconnaissance occurred | [RDP reconnaissance](attack-simulation/rdp-reconnaissance.png) | Authorized scanning identified the RDP service | An open port does not prove access or compromise |
| RDP authentication testing occurred | [Crowbar RDP attempt](attack-simulation/rdp-bruteforce-attempt.png) | Controlled credential testing was attempted | No valid credential or authenticated session was found |
| Mythic HTTP profile was configured | [Mythic C2 profile](attack-simulation/mythic-c2-profile.png) | HTTP C2 profile configuration existed | Does not prove an endpoint callback |
| Apollo payload creation was configured | [Payload creation](attack-simulation/mythic-payload-creation.png) | Apollo payload build configuration was completed | Does not prove execution or successful delivery |
| A payload artifact appeared in Mythic | [Mythic payload](attack-simulation/mythic-payload.png) | The created payload was listed in the interface | No active callback, session, or post-exploitation is claimed |

## Detection Engineering

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| SSH and Windows failed-logon rules existed | [Rules overview](detections/detection-rules-overview.png) | Custom rules were present in Elastic Security | Presence does not prove logic quality or coverage |
| SSH threshold settings were captured | [SSH rule definition](detections/ssh-rule-definition.png) | The lab SSH rule used a threshold of five grouped by source IP and username | Lab validation settings require normal-traffic tuning |
| SSH rule executed | [SSH rule execution](detections/ssh-rule-execution.png) | The rule ran successfully on its schedule | Successful execution does not mean useful alert precision |
| SSH alert was generated | [SSH alert](detections/ssh-alert.png) | The controlled SSH activity produced an alert | Alert alone does not prove malicious intent |
| Windows failed-logon settings were captured | [RDP rule definition](detections/rdp-rule-definition.png) | The lab rule used Event ID `4625`, threshold two, grouped by source IP and username | Event `4625` is not RDP-specific without validating Logon Type `10` |
| Windows failed-logon rule executed | [RDP rule execution](detections/rdp-rule-execution.png) | The rule ran successfully on its schedule | Does not demonstrate production-quality tuning |
| A related alert was generated | [RDP alert details](detections/rdp-alert-details.png) | A medium-severity, risk-score `47` alert was captured | Does not prove successful RDP authentication |
| Alert volume was visible | [Alerts overview](detections/alerts-overview.png) | Captured alert distribution and high RDP-rule volume | High volume indicates tuning and suppression work remains |

## Investigation

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| An SSH event was reviewed in detail | [SSH event detail](investigations/ssh-event-detail.png) | Event fields, source, user, process, location context, and outcome were inspected | GeoIP is enrichment, not identity proof |
| The analyst pivoted on user and source | [User/source pivot](investigations/ssh-user-source-pivot.png) | The dataset was narrowed using investigation fields | A filtered view is not a complete incident timeline |
| Failed SSH activity was isolated | [Failed-auth analysis](investigations/ssh-failed-auth-analysis.png) | Repeated failed authentication events were reviewed | Does not prove credential compromise |

The PDF-derived evidence identifies myVm (`10.1.0.5`) as the controlled SSH source and Ubuntu (`10.1.0.4`) as the target. Kali is used for the controlled Windows/RDP test, not the SSH test.

## Automation and Incident Tracking

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| Elastic could reach the ticket API | [Connector test](automation/osticket-connector-test-success.png) | The osTicket connector test returned success | Does not test retries, failure handling, or secret rotation |
| Security tickets existed in osTicket | [Ticket list](automation/osticket-ticket-list.png) | SSH and RDP-labelled incidents were present | Does not prove assignment, SLA, or complete closure workflow |
| An RDP-labelled ticket was created | [RDP API ticket](automation/rdp-api-ticket.png) | Alert details were represented in an osTicket incident | Ticket content inherits the RDP-rule specificity limitation |
| An SSH ticket was created | [SSH API ticket](automation/ssh-api-ticket.png) | SSH alert details were represented in an osTicket incident | Does not prove downstream response actions |

## Dashboards

| Claim | Evidence | What it proves | Boundary |
| --- | --- | --- | --- |
| A SOC dashboard was assembled | [Monitoring dashboard](dashboards/elk-soc-monitoring-dashboard.png) | A dashboard combined SSH failure trend and top-source panels | Current dashboard coverage is SSH-focused |
| SSH failures were trended | [Failures over time](dashboards/ssh-failures-over-time.png) | Failed SSH events were visualized across time | Does not measure detection quality by itself |
| Top SSH sources were ranked | [Top source IPs](dashboards/top-source-ip-visualization.png) | High-volume source addresses were visualized | Source IP alone does not establish actor identity |

## Evidence Status Summary

| Outcome | Status |
| --- | --- |
| Azure VM and VNet topology | Confirmed by screenshots |
| Windows and Ubuntu telemetry in Elastic | Confirmed by screenshots |
| SSH and Windows failed-logon alerts | Confirmed by screenshots |
| SSH investigation pivots | Confirmed by screenshots |
| Elastic-to-osTicket connector and tickets | Confirmed by screenshots |
| Successful RDP authentication | **Not confirmed** |
| Active Mythic/Apollo callback | **Not confirmed** |
| Production-ready detection tuning | **Not demonstrated** |
| Reproducible configuration exports | **Not yet included** |

## Publication and Handling

- Public architecture files intentionally omit public IPs, credentials, tokens, callback keys, and personal identifiers.
- Historical screenshots may contain retired lab identifiers and should be reviewed before reuse outside this portfolio.
- Redaction should use opaque replacement boxes, not blur, while preserving event IDs, timestamps, result fields, and other analytical context.
- No screenshot should be treated as proof beyond the boundary stated in this index.
- The seven long-form PDF evidence sets remain private supporting material; the repository uses selected screenshots for recruiter review.
