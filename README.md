# Azure ELK Security Lab

A hands-on Security Operations Center (SOC) lab built in Microsoft Azure using Elastic Stack, Windows Sysmon and Defender telemetry, Ubuntu authentication logs, attack simulation, detection engineering, incident investigation, dashboards, and automated incident ticketing.

## Project Overview

This project demonstrates an end-to-end SOC monitoring and investigation workflow:

**Attack Simulation → Telemetry Collection → Detection → Alert Triage → Investigation → Incident Ticketing → Security Monitoring**

The lab was built to practice practical SOC analyst activities, including:

- Collecting and analyzing Windows and Linux security telemetry
- Monitoring authentication activity
- Investigating RDP and SSH brute-force activity
- Creating and validating security detection rules
- Analyzing endpoint process and security events
- Investigating alerts using Elastic
- Generating incidents through automated ticketing
- Building dashboards for security monitoring
- Documenting investigation evidence and findings

## Architecture

![Azure ELK Security Lab Architecture](diagrams/Diagram.png)

## SOC Workflow

```text
Attack Simulation
       ↓
Endpoint / Network Activity
       ↓
Telemetry Collection
       ↓
Elastic
       ↓
Detection Rules
       ↓
Security Alert
       ↓
Alert Triage
       ↓
Investigation
       ↓
Incident Ticket
       ↓
Dashboard / Reporting
