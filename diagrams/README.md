# Architecture Diagram

This evidence-backed diagram documents the Azure topology and the confirmed SOC workflow without exposing public IPs, credentials, tokens, callback keys, or personal identifiers.

## Azure ELK SOC Lab Architecture

![Azure ELK SOC Lab Architecture](Diagram.png)

[Open the editable SVG](Diagram.svg)

## Verified Architecture Facts

- `elaskiba-vnet` (`10.0.0.0/16`, Korea Central) contains Elaskiba (`10.0.0.4`) and Fleet Server (`10.0.0.5`).
- `ubuntu-vnet` (`10.1.0.0/16`, Japan West) contains Ubuntu (`10.1.0.4`) and myVm/osTicket (`10.1.0.5`).
- `win-vnet` (`10.2.0.0/16`, Japan East) contains the Windows endpoint (`10.2.0.4`) and Mythic (`10.2.0.5`).
- The evidenced peerings are `elaskiba-vnet ↔ ubuntu-vnet` and `elaskiba-vnet ↔ win-vnet`. No direct spoke-to-spoke peering is claimed.
- Elastic Agents send endpoint telemetry to Elasticsearch. Fleet Server manages enrollment, policies, integrations, and agent health; it is not the telemetry-analysis engine.
- The PDF evidence shows controlled SSH failures from myVm (`10.1.0.5`) to Ubuntu (`10.1.0.4`) and controlled Kali RDP reconnaissance/failed-authentication testing against Windows.
- Elastic alerts generated osTicket incidents through a webhook/API workflow.
- Mythic HTTP C2 and Apollo payload configuration are confirmed; an active callback or session is not confirmed.

## Line Semantics

- Solid green: endpoint telemetry data plane
- Dashed blue: Fleet management/control plane
- Solid red: alert-to-ticket webhook/API
- Solid orange: controlled authentication testing
- Dashed gray: configured or intended path with an unconfirmed outcome
