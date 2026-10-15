# Day 4 — Practical Windows Security Log Investigation

## Objective
Investigate Windows Security events, collect evidence, and document findings without assuming that every event is malicious.

## Investigation 1 — Successful Service Logon

- **Tool:** Windows Event Viewer
- **Log:** Windows Logs → Security
- **Event ID:** 4624
- **Account:** SYSTEM
- **Domain:** NT AUTHORITY
- **Logon type:** 5 — Service
- **Process:** `C:\Windows\System32\services.exe`
- **Authentication package:** Negotiate
- **Source address:** Not recorded
- **Elevated token:** Yes

**Analysis:** Windows recorded a successful service logon for the built-in SYSTEM account. Logon type 5 indicates a service logon. The event is consistent with normal Windows service activity.

**Conclusion:** No malicious activity is established by this event alone.

## Investigation 2 — Failed Windows Logon

- **Event ID:** 4625
- **Account:** hp
- **Logon type:** 2 — Interactive
- **Failure status:** `0xC000006D`
- **Failure substatus:** `0xC000006A`
- **Process:** `C:\Windows\System32\svchost.exe`
- **Source address:** `127.0.0.1`
- **Workstation:** `DESKTOP-SK495ID`

**Analysis:** Windows recorded a failed logon for account hp. The substatus indicates an incorrect password. The source address is the local loopback address, not an external IP address.

**Conclusion:** This is consistent with a local authentication failure. One failed logon does not prove an attack. Check surrounding events for repeated failures or other suspicious activity.

## Investigation 3 — Compare Successful and Failed Logons

- **Successful event:** 4624 — SYSTEM account, logon type 5.
- **Failed event:** 4625 — hp account, logon type 2, incorrect password.

**Analysis:** The events involve different accounts and logon types. Do not assume they are related without comparing timestamps and other evidence.

**Conclusion:** Comparing successful and failed logons helps an analyst understand authentication activity and identify patterns that need further investigation.

## Investigation 4 — Windows Process Creation

- **Event ID:** 4688
- **New process:** `C:\Windows\System32\services.exe`
- **Creator process:** `C:\Windows\System32\wininit.exe`
- **New process ID:** `0x2a8`
- **Creator process ID:** `0x3ac`
- **Security context:** SYSTEM
- **Token elevation type:** TokenElevationTypeDefault (1)
- **Mandatory label:** System Mandatory Level
- **Command line:** Not recorded in this event

**Analysis:** Windows recorded the creation of services.exe by wininit.exe. This relationship is consistent with normal Windows startup activity. The command line was not recorded in this event.

**Conclusion:** The available evidence does not establish malicious activity. Further checks could include the timestamp, executable signature, file path, and related process events.

## Key Lessons

1. Event ID 4624 records a successful logon.
2. Event ID 4625 records a failed logon.
3. Event ID 4688 records process creation.
4. Logon types, account names, process paths, and timestamps provide investigation context.
5. A single event does not prove an attack.
6. A SOC analyst should record evidence, explain the reasoning, and recommend further checks when necessary.

## Tools Used

- Windows Event Viewer
- Windows Security Logs

**Status:** Day 4 practical investigation documented.
