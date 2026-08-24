# Telemetry Collection

The lab collects security-relevant telemetry from both Windows and Linux endpoints and sends it into the Elastic environment.

## Windows Telemetry

The Windows endpoint (`win`) is configured for security telemetry collection including:

- Sysmon
- Windows Event Logs
- Windows Defender events
- Elastic Agent

## Linux Telemetry

The Ubuntu endpoint (`10.1.0.4`) is configured for:

- System logs
- Authentication logs
- SSH logs
- Elastic Agent

## Telemetry Pipeline

```text
Windows / Ubuntu
       │
       ▼
Elastic Agent
       │
       ▼
Elasticsearch
       │
       ▼
Kibana / Elastic Security
