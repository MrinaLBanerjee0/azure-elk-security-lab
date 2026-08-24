# Azure ELK Security Lab

A hands-on SOC lab built using Microsoft Azure, Elastic Stack, endpoint
telemetry, attack simulation, and incident ticketing.

## Project Overview

This project was built to develop practical skills in security monitoring,
log analysis, threat detection, incident investigation, and network
troubleshooting.

The lab uses multiple Azure virtual machines across different regions,
Connected through Azure Virtual Network peering. The project involved
Troubleshooting connectivity between systems, configuring the Elastic
Stack, collecting endpoint telemetry, investigating security logs,
Creating alerts and dashboards, and integrating incident ticketing.

The lab also provided practical experience in understanding how the
Different components work together as part of a security monitoring and
Investigation workflow.

## Architecture

![Azure ELK Security Lab Architecture](diagrams/Diagram.png)


## Lab Environment

| Component | Private IP | VNet | Region | Role |
|---|---|---|---|---|
| Elaskiba | 10.0.0.4 | elaskiba-vnet | Korea Central | Elasticsearch + Kibana |
| Fleet Server | 10.0.0.5 | elaskiba-vnet | Korea Central | Elastic Fleet / Agent Management |
| Ubuntu | 10.1.0.4 | ubuntu-vnet | Japan West | Linux Log Source |
| myVm | 10.1.0.5 | ubuntu-vnet | Japan West | osTicket / Ticketing |
| win | 10.2.0.4 | win-vnet | Japan East | Windows Log Source |
| Mythic C2 Server | 10.2.0.5 | win-vnet | Japan East | C2 / Attack Simulation |
