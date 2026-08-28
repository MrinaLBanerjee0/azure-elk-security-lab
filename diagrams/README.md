# Architecture Diagram

The architecture diagram shows the overall design of the Azure-based SOC lab and the flow between infrastructure, endpoints, Elastic components, attack simulation, detection, investigation, and automated ticketing.

## Azure SOC Lab Architecture

![Azure ELK SOC Architecture](Diagram.png)

The diagram shows:

- Azure hub-and-spoke network architecture
- Elasticsearch and Kibana
- Elastic Fleet Server
- Windows and Ubuntu endpoints
- Mythic attack-simulation environment
- osTicket ticketing server
- Telemetry flow into Elastic
- Detection, investigation, and automated ticketing workflow
