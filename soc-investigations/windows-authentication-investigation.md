# Windows Authentication Investigation

## Overview

This project documents a simulated Security Operations Center (SOC) investigation involving repeated Windows authentication failures followed by a successful privileged login.

The investigation began with authentication telemetry and expanded into endpoint activity to determine what occurred after the successful login. PowerShell was used to filter, group, sort, and correlate events from multiple synthetic data sources.

> **Lab Notice:** All accounts, IP addresses, hostnames, events, and activity documented in this investigation are synthetic training data created for cybersecurity practice. This project does not represent a real security incident.

## Investigation Scenario

A monitoring alert identified multiple failed Windows authentication attempts against an internal workstation followed by successful authentication activity.

The investigation focused on determining:

- Which accounts generated failed authentication attempts
- Which source systems were involved
- Whether successful authentication followed the failures
- What type of authentication occurred
- What activity occurred after successful authentication
- Whether the available evidence warranted escalation

## Data Sources

Two synthetic datasets were analyzed during the investigation:

### Authentication Telemetry

`windows-auth-events.csv`

The authentication dataset contained Windows-style authentication events including:

- Event ID 4624 — Successful logon
- Event ID 4625 — Failed logon
- Account
- Source IP address
- Workstation
- Logon type
- Authentication status

### Endpoint Telemetry

`windows-endpoint-events.csv`

The endpoint dataset contained simulated post-authentication activity including:

- Process execution
- Command-line activity
- Host information
- Network share access attempts
- Successful network share access

## Initial Triage

Failed authentication events were filtered and grouped by account using PowerShell.

```powershell
## Authentication Analysis

### Failed Authentication Review

The initial triage identified repeated failed authentication attempts involving two accounts:

- `administrator` — 7 failed authentication attempts
- `m.roberts` — 5 failed authentication attempts

Rather than treating the number of failures alone as evidence of malicious activity, each account was investigated separately to examine its source, timing, logon type, and subsequent authentication activity.

### Administrator Account

PowerShell was used to isolate authentication activity associated with the `administrator` account:

```powershell
$events |
Where-Object {$_.Account -eq 'administrator'} |
Select-Object Timestamp, EventID, Account, SourceIP, Workstation, LogonType, Status |
Format-Table -AutoSize
```

The investigation identified:

- Seven failed authentication attempts between `09:44:11` and `09:44:37`
- All attempts originated from `10.10.50.91`
- All events involved `WS-ADMIN-02`
- All attempts used Logon Type 10 (RemoteInteractive)
- A successful authentication occurred at `09:45:02`

The successful authentication occurred approximately 25 seconds after the final failed attempt.

Because the activity involved a privileged account and RemoteInteractive authentication, the account was prioritized for additional investigation.

The authentication pattern was considered suspicious, but the authentication events alone did not establish that the account had been compromised.

### M. Roberts Account

The same process was used to investigate the `m.roberts` account:

```powershell
$events |
Where-Object {$_.Account -eq 'm.roberts'} |
Select-Object Timestamp, EventID, Account, SourceIP, Workstation, LogonType, Status |
Format-Table -AutoSize
```

The investigation identified:

- Five failed authentication attempts between `09:05:41` and `09:06:09`
- All attempts originated from `10.10.30.44`
- All events involved `WS-ENG-12`
- All attempts used Logon Type 3 (Network)
- A successful authentication occurred at `09:07:26`

This activity could be consistent with user error, cached credentials, an application or service repeatedly attempting authentication, or unauthorized authentication attempts.

The available evidence was insufficient to determine the cause.

### Initial Prioritization

Both accounts warranted investigation, but the `administrator` activity was prioritized because it combined:

- A privileged account
- Repeated authentication failures
- RemoteInteractive authentication
- A successful authentication shortly after the failures

This prioritization did not establish malicious activity. It identified the authentication sequence that presented the greater investigative concern based on the available evidence.
$events |
Where-Object {$_.EventID -eq '4625'} |
Group-Object Account |
Sort-Object Count -Descending |
Select-Object Count, Name |
Format-Table -AutoSize
