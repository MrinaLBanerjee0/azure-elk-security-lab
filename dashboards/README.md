# Security Dashboards

Kibana dashboards and visualizations were created to provide an analyst-facing view of authentication activity and identify notable sources of security events.

## ELK SOC Monitoring Dashboard

![ELK SOC Monitoring Dashboard](../evidence/dashboards/elk-soc-monitoring-dashboard.png)

The SOC monitoring dashboard provides a consolidated view of SSH authentication failures over time and the top source IP addresses.

## SSH Failures Over Time

![SSH Failures Over Time](../evidence/dashboards/ssh-failures-over-time.png)

This visualization shows SSH authentication failure activity over time, helping identify periods of increased authentication activity.

## Top Source IPs

![Top Source IP Visualization](../evidence/dashboards/top-source-ip-visualization.png)

This visualization shows the most frequent source IP addresses associated with the observed SSH authentication events.

## Analyst Use

These visualizations support:

- Monitoring authentication activity
- Identifying spikes in failed SSH authentication
- Identifying prominent source IP addresses
- Providing context during security investigations
