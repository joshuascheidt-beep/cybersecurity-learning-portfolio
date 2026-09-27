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
$events |
Where-Object {$_.EventID -eq '4625'} |
Group-Object Account |
Sort-Object Count -Descending |
Select-Object Count, Name |
Format-Table -AutoSize
