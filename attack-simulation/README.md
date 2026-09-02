# Attack Simulation

Controlled activity was performed to generate security telemetry and exercise the SOC workflow.

## RDP Reconnaissance and Authentication Testing

![RDP Reconnaissance](../evidence/attack-simulation/rdp-reconnaissance.png)

Network reconnaissance identified TCP port `3389` on the Windows target.

![RDP Brute-Force Attempt](../evidence/attack-simulation/rdp-bruteforce-attempt.png)

Crowbar performed controlled RDP credential testing. The captured result showed no valid credential and no authenticated RDP session.

This proves the RDP test occurred. It does not make the separate Event ID `4625` threshold rule RDP-specific, because that exported query did not validate Logon Type `10`.

## Mythic and Apollo Configuration

![Mythic Payload Creation](../evidence/attack-simulation/mythic-payload-creation.png)

![Mythic C2 Profile](../evidence/attack-simulation/mythic-c2-profile.png)

![Mythic Payload](../evidence/attack-simulation/mythic-payload.png)

The evidence proves that an HTTP C2 profile was configured and an Apollo Windows payload was created and listed in Mythic. It does not prove payload execution, delivery, an active callback, a session, command execution, or post-exploitation.

## Evidence Boundary

```text
Confirmed: RDP test attempt; Mythic profile; Apollo payload build
Not confirmed: valid RDP credential; Mythic callback or session
```
