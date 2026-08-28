# Attack Simulation

Controlled attack simulations were performed to generate realistic security activity for the SOC lab and validate telemetry, detections, investigations, and automated response workflows.

## RDP Reconnaissance

Network reconnaissance was performed against the Windows endpoint to identify the exposed RDP service before attempting authentication attacks.

![RDP Reconnaissance](../evidence/attack-simulation/rdp-reconnaissance.png)

This evidence shows the reconnaissance activity performed against the RDP target.

## RDP Brute-Force Attempt

A controlled RDP brute-force attempt was performed using Crowbar against the Windows endpoint.

![RDP Brute-Force Attempt](../evidence/attack-simulation/rdp-bruteforce-attempt.png)

The attempt was **unsuccessful** and did not establish authenticated access. It was used to generate authentication-related activity for SOC detection and investigation testing.

## Mythic Payload Creation

A Windows payload was created in the Mythic C2 platform as part of the controlled attack-simulation environment.

![Mythic Payload Creation](../evidence/attack-simulation/mythic-payload-creation.png)

This evidence shows the payload creation process in Mythic.

## Mythic C2 Profile

An HTTP-based C2 profile was configured in Mythic for the simulation environment.

![Mythic C2 Profile](../evidence/attack-simulation/mythic-c2-profile.png)

This shows the configured HTTP C2 profile used by the payload.

## Mythic Payload

![Mythic Payload](../evidence/attack-simulation/mythic-payload.png)

This shows the created Windows payload within Mythic.

## Purpose

The simulations were used to generate security activity that could be observed through endpoint telemetry and evaluated through the SOC workflow:

```text
Attack Simulation
        ↓
Telemetry Collection
        ↓
Detection
        ↓
Alert
        ↓
Investigation
        ↓
Automated Ticketing
