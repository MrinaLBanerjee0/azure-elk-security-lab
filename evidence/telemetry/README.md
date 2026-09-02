# Telemetry Evidence

## Fleet Enrollment

![Fleet Agents](fleet-agents.png)

Shows the `win`, `ubuntu`, and `fleet` agents healthy at capture time only.

## Windows Integrations

![Windows Telemetry Integrations](windows-telemetry-integrations.png)

Shows Elastic Defend, System, `win-defender`, and `win-sysmon` integrations assigned to the Windows policy.

## Ubuntu Policy

![Ubuntu Fleet Policy](ubuntu-fleet-policy.png)

Shows the Ubuntu `soc-linux` policy with the System integration. The applied-policy artifact supplies the exact paths and datasets.

## Sysmon Process Creation

![Sysmon Process Creation](sysmon-process-create.png)

Shows one Sysmon Event ID `1` process-create event. It does not prove complete Sysmon event coverage.

## Windows Defender

![Windows Defender Events](windows-defender-events.png)

Shows Defender Event ID `5007` records searchable in Elastic. This observed-data screenshot is separate from the Fleet export, which configures only `1116`, `1117`, and `5001`.

## Ubuntu SSH

![Ubuntu SSH Events](ubuntu-ssh-events.png)

Shows SSH authentication events collected from the Ubuntu endpoint.
