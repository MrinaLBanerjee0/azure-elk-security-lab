# Architecture Diagram

This diagram separates physical/logical topology from security activity and SOC outcomes. It is designed to show what the evidence confirms without implying unsupported connectivity, compromise, or command-and-control success.

![Azure ELK SOC Lab Architecture](Diagram.png)

[Open the editable SVG](Diagram.svg)

## Verified Facts

- `elaskiba-vnet` (`10.0.0.0/16`, Korea Central) contains Elaskiba (`10.0.0.4`) and Fleet Server (`10.0.0.5`).
- `ubuntu-vnet` (`10.1.0.0/16`, Japan West) contains Ubuntu (`10.1.0.4`) and myVm/osTicket (`10.1.0.5`).
- `win-vnet` (`10.2.0.0/16`, Japan East) contains the Windows endpoint (`10.2.0.4`) and Mythic (`10.2.0.5`).
- The evidence supports only `elaskiba-vnet ↔ ubuntu-vnet` and `elaskiba-vnet ↔ win-vnet` peerings. Azure VNet peering is non-transitive; no direct spoke-to-spoke connectivity is claimed.
- Elastic Agents send Windows and Linux telemetry to Elasticsearch. Fleet Server provides the separate management/control plane.
- Controlled SSH failures originated from myVm (`10.1.0.5`) and targeted Ubuntu (`10.1.0.4`).
- Kali performed controlled RDP reconnaissance and unsuccessful authentication testing against Windows.
- The Windows validation rule captured Event ID `4625`; RDP-specific production logic would additionally require Logon Type `10`.
- Elastic alerts generated basic osTicket tickets through a webhook/API workflow; assignment and closure are not demonstrated.
- Mythic HTTP C2 profile and Apollo payload creation are confirmed. The intended callback is endpoint-initiated, but no active callback or session is confirmed.

## Visual Semantics

| Style | Meaning |
| --- | --- |
| Purple, double-ended | Verified VNet peering |
| Green, solid | Endpoint telemetry data plane |
| Blue, dashed | Agent-initiated Fleet management exchange |
| Orange, solid | Controlled test activity |
| Red, solid | Alert-to-ticket webhook/API |
| Gray, dashed | Configured or intended path with unconfirmed outcome |

Public IPs, credentials, tokens, callback keys, and personal identifiers are intentionally omitted.
