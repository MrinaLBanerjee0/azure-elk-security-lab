# Exported Configuration Artifacts

These files were exported from the lab or, in the Sysmon case, preserved from the third-party configuration actually deployed. Screenshots remain under `evidence/`; this directory is for configuration and saved-object artifacts. They are historical lab evidence, not drop-in production templates.

| Artifact | Type | Exact boundary |
| --- | --- | --- |
| [`detections/custom-rules-and-osticket-connector.sanitized.ndjson`](detections/custom-rules-and-osticket-connector.sanitized.ndjson) | Elastic Security rule export plus webhook connector | Both historical rule objects and actions are unchanged except the connector API-key value is `REDACTED` |
| [`dashboards/elk-soc-monitoring-dashboard.ndjson`](dashboards/elk-soc-monitoring-dashboard.ndjson) | Kibana saved-object export | Real `ELK SOC Monitoring Dashboard`, `logs-*` data view, and two SSH panels |
| [`fleet/windows-applied-policy.yml`](fleet/windows-applied-policy.yml) | Elastic Agent applied-policy snapshot | Proves Windows/System/Sysmon/Defender/Elastic Defend inputs; not a clean Fleet import package |
| [`fleet/ubuntu-applied-policy.yml`](fleet/ubuntu-applied-policy.yml) | Elastic Agent applied-policy snapshot | Proves Linux paths and `system.auth`/`system.syslog`; not a clean Fleet import package |
| [`sysmon/sysmon-modular-balanced.xml`](sysmon/sysmon-modular-balanced.xml) | Sysmon configuration | Third-party Olaf Hartong Sysmon Modular balanced configuration; not authored in this project |

## Historical Rule Logic

### `soc ssh brute force`

- Query: `system.auth.ssh.event: * and agent.name: ubantu and system.auth.ssh.event: Failed and user.name : Mrinal`
- Threshold: `5`, grouped by `user.name` and `source.ip`
- Schedule: every `5m`, lookback `now-10m`

### `win Rdp brute force`

- Historical name and description are preserved.
- Query: `event.code: 4625 and agent.name: win and user.name: Mrinal`
- Threshold: `2`, grouped by `source.ip` and `user.name`
- Schedule: every `30s`, lookback `now-330s`
- No Logon Type `10` condition exists, so the exported rule is not RDP-specific.

## Reuse Boundaries

- `ubantu` is the historical misspelled Elastic Agent name preserved in the SSH query; it must match an actual agent name or be changed before reuse.
- `Mrinal`, `win`, the thresholds, schedules, and grouping fields are lab-specific validation values, not general production recommendations.
- The connector URL points to the private lab host and the `REDACTED` API-key placeholder is intentionally nonfunctional.
- The Fleet YAML files are applied-policy snapshots for evidence and review, not clean Fleet import packages.

The raw Windows rule description says "administrator via RDP," but the preserved query filters `user.name: Mrinal`, does not validate Logon Type `10`, and is therefore neither administrator-specific nor RDP-specific. This inconsistency is historical and was not silently corrected in the export.

## Credential Sanitization

The source NDJSON placed the osTicket API key in `attributes.config.headers.x-API-key`, not in the connector `secrets` object. That one value was replaced with `REDACTED`. No query, threshold, schedule, grouping, rule name, description, action body, or connector URL was changed.

## Sysmon Attribution

The Sysmon file identifies itself as the balanced, medium-verbosity generated output from [Olaf Hartong's Sysmon Modular project](https://github.com/olafhartong/sysmon-modular). It is a starting configuration with acknowledged blind spots and requires environment-specific tuning. See [`THIRD-PARTY-NOTICES.md`](../THIRD-PARTY-NOTICES.md).
