# Evidence Index

I kept the main screenshots here so the project claims can be checked quickly. The third column says what I used each screenshot for, and the last column calls out the main thing the screenshot cannot establish on its own.

[Back to the project README](../README.md) · [Full investigation report](../SOC-INVESTIGATION-REPORT.md)

## Infrastructure

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| Six Azure VMs were used | [VM inventory](azure/azure-vm-inventory.png) | VM inventory and deployment context | VM presence alone does not prove service health |
| Hub-to-spoke peerings existed | [VNet peerings](azure/azure-vnet-peerings.png) | The two documented peerings involving `elaskiba-vnet` | I do not claim a direct `ubuntu-vnet ↔ win-vnet` peering |
| Elaskiba used the documented private network | [Elaskiba network](azure/elaskiba-network.png) | Elaskiba NIC, private IP, subnet, and VNet context | This does not show every route or NSG rule |
| myVm used the documented private network | [myVm network](azure/myvm-network.png) | myVm NIC, private IP, subnet, and VNet context | This does not prove direct connectivity to the Windows spoke |

## Telemetry and Fleet

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| Agents were enrolled and healthy | [Fleet agents](telemetry/fleet-agents.png) | `win`, `ubuntu`, and `fleet` were healthy when captured | A screenshot is only a point-in-time health check |
| Windows integrations were configured | [Windows integrations](telemetry/windows-telemetry-integrations.png) | The Windows policy included System, Defender, Sysmon, and Elastic Defend | Configuration alone does not prove every dataset had data |
| Sysmon process telemetry arrived | [Sysmon process create](telemetry/sysmon-process-create.png) | A Sysmon Event ID `1` process-create event was searchable | One event is not proof of complete Sysmon coverage |
| Defender telemetry arrived | [Defender events](telemetry/windows-defender-events.png) | Defender Event ID `5007` records were searchable | The policy export itself configures `1116`, `1117`, and `5001`; this is separate observed data |
| Ubuntu SSH events arrived | [Ubuntu SSH events](telemetry/ubuntu-ssh-events.png) | SSH authentication activity was ingested from Ubuntu | The log event does not tell me intent by itself |

Elastic Agents send the endpoint data to Elasticsearch. Fleet Server is used for enrollment, policies, integrations, actions, status, and health; I am not presenting it as the telemetry relay or analysis engine.

## Controlled attack simulation

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| RDP service reconnaissance occurred | [RDP reconnaissance](attack-simulation/rdp-reconnaissance.png) | Authorized scanning identified the RDP service | An open port is not proof of access or compromise |
| RDP authentication testing occurred | [Crowbar RDP attempt](attack-simulation/rdp-bruteforce-attempt.png) | Controlled credential testing was attempted | No valid credential or authenticated session was found |
| Mythic HTTP profile was configured | [Mythic C2 profile](attack-simulation/mythic-c2-profile.png) | An HTTP C2 profile existed | This does not prove an endpoint callback |
| Apollo payload creation was configured | [Payload creation](attack-simulation/mythic-payload-creation.png) | Apollo payload build configuration was completed | This does not prove execution or successful delivery |
| A payload artifact appeared in Mythic | [Mythic payload](attack-simulation/mythic-payload.png) | The created payload was listed in Mythic | I do not claim an active callback, session, or post-exploitation |

## Detection rule evidence

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| SSH and Windows failed-logon rules existed | [Rules overview](detections/detection-rules-overview.png) | Custom rules were present in Elastic Security | Rule presence says nothing about detection quality by itself |
| SSH threshold settings were captured | [SSH rule definition](detections/ssh-rule-definition.png) | Threshold `5`, grouped by source IP and username | These were lab settings and would need tuning against normal traffic |
| SSH rule executed | [SSH rule execution](detections/ssh-rule-execution.png) | The rule ran successfully on schedule | Successful execution does not mean good precision |
| SSH alert was generated | [SSH alert](detections/ssh-alert.png) | The controlled SSH activity produced an alert | An alert is not proof of malicious intent |
| Windows failed-logon settings were captured | [RDP rule definition](detections/rdp-rule-definition.png) | Event ID `4625`, threshold `2`, grouped by source IP and username | `4625` is not RDP-specific unless Logon Type `10` is checked |
| Windows failed-logon rule executed | [RDP rule execution](detections/rdp-rule-execution.png) | The rule ran successfully on schedule | This does not demonstrate production-quality tuning |
| A related alert was generated | [RDP alert details](detections/rdp-alert-details.png) | A medium-severity, risk-score `47` alert was captured | This is not proof of successful RDP authentication |
| Alert volume was visible | [Alerts overview](detections/alerts-overview.png) | The screenshot shows high alert volume from the historically named Windows rule | The volume itself shows that more tuning/suppression was needed |

## Investigation

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| An SSH event was reviewed in detail | [SSH event detail](investigations/ssh-event-detail.png) | Event fields, source, user, process, location context, and outcome were inspected | GeoIP is enrichment, not proof of actor identity |
| I pivoted on user and source | [User/source pivot](investigations/ssh-user-source-pivot.png) | The data was narrowed using investigation fields | A filtered view is not the whole incident timeline |
| Failed SSH activity was isolated | [Failed-auth analysis](investigations/ssh-failed-auth-analysis.png) | Repeated failed authentication events were reviewed | This does not prove credential compromise |

The supporting evidence identifies myVm (`10.1.0.5`) as the controlled SSH source and Ubuntu (`10.1.0.4`) as the target. Kali was used for the Windows/RDP test, not the SSH test.

## Automation and incident tracking

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| A webhook connector was configured | [Connector configuration](automation/osticket-connector-configuration.png) | Connector name, POST method, endpoint, and header configuration | The screenshot does not expose or validate the API-key value |
| Elastic could reach the ticket API | [Connector test](automation/osticket-connector-test-success.png) | The connector test returned success | This does not test retries, failure handling, or secret rotation |
| Security tickets existed in osTicket | [Ticket list](automation/osticket-ticket-list.png) | SSH and historically RDP-labelled tickets were present | It does not prove assignment, SLA, or closure workflow |
| A historically RDP-labelled ticket was created | [Windows API ticket](automation/rdp-api-ticket.png) | The API created a ticket with the historical rule name and basic investigation message | The ticket does not prove RDP specificity or enriched alert context |
| An SSH ticket was created | [SSH API ticket](automation/ssh-api-ticket.png) | The API created a ticket with the SSH rule name and basic message | This does not show enrichment or downstream response actions |

## Dashboards

| Claim | Screenshot | What I can verify from it | Important limit |
| --- | --- | --- | --- |
| A SOC dashboard was assembled | [Monitoring dashboard](dashboards/elk-soc-monitoring-dashboard.png) | The dashboard combined SSH failure trend and top-source panels | The current dashboard is mainly SSH-focused |
| SSH failures were trended | [Failures over time](dashboards/ssh-failures-over-time.png) | Failed SSH events were plotted across time | A chart by itself does not measure detection quality |
| Top SSH sources were ranked | [Top source IPs](dashboards/top-source-ip-visualization.png) | High-volume source addresses were visualized | Source IP alone does not identify an actor |

## Exported artifacts

The screenshots are not the only evidence in the repo. I also kept the configuration exports that were available from the lab:

- [Rules and connector export](../artifacts/detections/custom-rules-and-osticket-connector.sanitized.ndjson) — historical rule logic with the API-key value redacted.
- [Dashboard export](../artifacts/dashboards/elk-soc-monitoring-dashboard.ndjson) — Kibana saved objects using `logs-*`.
- [Windows applied policy](../artifacts/fleet/windows-applied-policy.yml) — Windows/System/Sysmon/Defender/Elastic Defend inputs.
- [Ubuntu applied policy](../artifacts/fleet/ubuntu-applied-policy.yml) — Linux log paths and datasets.
- [Sysmon configuration](../artifacts/sysmon/sysmon-modular-balanced.xml) — third-party balanced config attributed to Olaf Hartong/Sysmon Modular.

## What the final evidence set supports

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
| Exported configuration artifacts | **Included; Fleet policy files are evidence snapshots, not clean import packages** |

## Publication notes

- Public architecture files leave out public IPs, credentials, tokens, callback keys, and personal identifiers.
- Some historical screenshots contain retired lab identifiers, so I review them before reusing them elsewhere.
- When redaction is needed, I use opaque replacement rather than blur while keeping event IDs, timestamps, and analysis fields visible.
- I do not treat a screenshot as proof of more than the information visible in it.
- The seven long-form PDF evidence sets remain private supporting material; the public repository uses selected screenshots for review.
