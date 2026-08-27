# Attack Simulation Evidence

Screenshots documenting controlled attack-simulation activity used to generate security events for the SOC lab.

## RDP Attack Simulation

### RDP Reconnaissance

![RDP Reconnaissance](rdp-reconnaissance.png)

Network reconnaissance was performed against the Windows RDP target before the authentication attack.

### RDP Brute-Force Attempt

![RDP Brute-Force Attempt](rdp-bruteforce-attempt.png)

A Crowbar RDP brute-force attempt was performed against the Windows endpoint. The attempt was unsuccessful and did not establish authenticated access.

## Mythic C2 Simulation

### Mythic Payload Creation

![Mythic Payload Creation](mythic-payload-creation.png)

A Windows payload was created in the Mythic C2 platform for controlled attack simulation.

### Mythic C2 Profile

![Mythic C2 Profile](mythic-c2-profile.png)

An HTTP-based C2 profile was configured for the attack-simulation environment.

### Mythic Payload

![Mythic Payload](mythic-payload.png)

The created payload is shown in the Mythic interface as part of the controlled C2 simulation.
