# Telemetry Collection

The SOC lab collects security-relevant telemetry from both Windows and Linux endpoints and forwards the events to the Elastic environment for centralized monitoring and detection.

## Windows Telemetry

The Windows endpoint (`win`) is configured to collect security telemetry including:

- Sysmon
- Windows Event Logs
- Microsoft Defender events
- Elastic Agent

### Sysmon

Sysmon provides detailed Windows endpoint telemetry such as process creation and other system activity used for security investigation and detection.

![Sysmon Process Creation](../evidence/telemetry/sysmon-process-create.png)

This evidence shows a Sysmon Process Create event (Event ID 1).

### Windows Defender

Microsoft Defender operational events are collected through the Elastic Agent and made available in Elastic for security monitoring.

![Windows Defender Events](../evidence/telemetry/windows-defender-events.png)

This evidence shows Windows Defender operational events collected from the Windows endpoint.

## Ubuntu Telemetry

The Ubuntu endpoint (`10.1.0.4`) is configured to collect:

- System logs
- Authentication logs
- SSH logs
- Elastic Agent

### SSH Authentication

![Ubuntu SSH Events](../evidence/telemetry/ubuntu-ssh-events.png)

This evidence shows SSH authentication events collected from the Ubuntu endpoint and ingested into Elastic.

## Elastic Agent

Elastic Agent is used to collect and forward endpoint telemetry into the Elastic environment.

![Fleet Agents](../evidence/telemetry/fleet-agents.png)

The Fleet view shows the enrolled agents and their health status.

### Windows Telemetry Integrations

![Windows Telemetry Integrations](../evidence/telemetry/windows-telemetry-integrations.png)

This shows the Windows telemetry integrations configured in the Elastic Agent policy.

## Telemetry and Fleet Management Flow

Fleet management and endpoint telemetry use separate logical paths.

### Telemetry Data Plane

```text
Windows endpoint                         Ubuntu endpoint
  ├── Windows Security events              ├── Linux system logs
  ├── Sysmon events                        ├── Authentication logs
  └── Microsoft Defender events            └── SSH events
             │                                        │
             ▼                                        ▼
    Windows Elastic Agent                     Ubuntu Elastic Agent
             │                                        │
             └───────────────┬────────────────────────┘
                             │ Endpoint telemetry
                             ▼
                       Elasticsearch
                             │
                             ▼
                  Kibana / Elastic Security
                    ├── Search and dashboards
                    ├── Detection rules and alerts
                    └── Investigation
```

### Fleet Management Plane

```text
Kibana Fleet UI
      │ Stores policies and actions
      ▼
Elasticsearch Fleet indices
      ▲
      │ Fleet Server monitors and updates Fleet state
      ▼
Fleet Server
      ▲
      │ Agents initiate HTTPS enrollment and check-ins
      ▼
Windows and Ubuntu Elastic Agents
      └── Policy retrieval, actions, status, health,
          and lifecycle management
```

Fleet Server manages Elastic Agents and their policies. It is not the endpoint-telemetry relay, the telemetry store, or the security-analysis engine.
