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
## Cross-Source Event Correlation

After identifying the suspicious authentication sequence involving the `administrator` account, additional endpoint telemetry was reviewed to determine what activity occurred after the successful login.

Two synthetic telemetry sources were correlated:

- Windows authentication events
- Endpoint process and resource-access events

PowerShell was used to normalize both datasets into a common timeline.

### PowerShell Correlation

```powershell
$authTimeline = $events |
Where-Object {$_.Account -eq 'administrator'} |
Select-Object @{Name='Timestamp';Expression={[datetime]$_.Timestamp}},
              @{Name='Source';Expression={'Authentication'}},
              @{Name='Activity';Expression={
                  if ($_.EventID -eq '4625') {'Failed Logon'}
                  elseif ($_.EventID -eq '4624') {'Successful Logon'}
              }},
              @{Name='Details';Expression={
                  "SourceIP=$($_.SourceIP); LogonType=$($_.LogonType)"
              }}

$endpointTimeline = $endpoint |
Where-Object {$_.Account -eq 'administrator'} |
Select-Object @{Name='Timestamp';Expression={[datetime]$_.Timestamp}},
              @{Name='Source';Expression={'Endpoint'}},
              @{Name='Activity';Expression={$_.Action}},
              @{Name='Details';Expression={
                  if ($_.CommandLine) {
                      "$($_.Process) -> $($_.CommandLine)"
                  } else {
                      "$($_.Process) -> $($_.Target)"
                  }
              }}

$timeline = $authTimeline + $endpointTimeline

$timeline |
Sort-Object Timestamp |
Format-Table Timestamp, Source, Activity, Details -AutoSize
```

### Correlated Timeline

| Time | Source | Activity | Details |
|---|---|---|---|
| 09:44:11–09:44:37 | Authentication | Failed Logons | Seven failures from `10.10.50.91`, Logon Type 10 |
| 09:45:02 | Authentication | Successful Logon | `administrator` authenticated from `10.10.50.91` using Logon Type 10 |
| 09:45:18 | Endpoint | Process Start | `powershell.exe` |
| 09:45:31 | Endpoint | Process Start | `whoami.exe` |
| 09:45:36 | Endpoint | Process Start | `hostname.exe` |
| 09:45:44 | Endpoint | Process Start | `net.exe` with `net view` |
| 09:46:03 | Endpoint | Share Access Attempt | `\\FILESERVER01\Finance` |
| 09:46:17 | Endpoint | Share Access Success | `\\FILESERVER01\Finance` |

### Timeline Analysis

The correlated timeline showed that a successful privileged RemoteInteractive authentication was followed 16 seconds later by the launch of PowerShell.

The session subsequently executed:

- `whoami` — identifies the current user/security context
- `hostname` — identifies the local computer
- `net view` — can be used to identify computers or shared resources available on the network

The sequence was followed by attempted and successful access to the `\\FILESERVER01\Finance` network share.

The activity from the first failed authentication attempt through successful Finance share access occurred within approximately two minutes.

Individually, the observed commands can have legitimate administrative uses. However, their proximity to repeated authentication failures, a successful privileged RemoteInteractive login, and subsequent network-share access increased the investigative significance of the overall sequence.

## Analyst Assessment

The activity was classified as **suspicious and requiring escalation for further investigation**.

The assessment was based on the correlation of multiple events rather than any single indicator:

1. Repeated failed authentication attempts targeted a privileged account.
2. A successful RemoteInteractive authentication followed the failures.
3. PowerShell launched shortly after authentication.
4. System and network discovery commands were executed.
5. The session subsequently accessed a potentially sensitive Finance network share.

The available evidence does **not** establish that the session was unauthorized, that sensitive financial files were opened, or that data was copied or exfiltrated.

Additional investigation would be required before classifying the activity as a confirmed security incident.

## Recommended Next Steps

Further investigation should include:

- Determine whether `10.10.50.91` is an authorized administrative workstation.
- Verify whether the administrator account owner initiated the RemoteInteractive session.
- Review historical authentication activity for the administrator account.
- Review endpoint telemetry from `WS-ADMIN-02`.
- Review file-access auditing for `\\FILESERVER01\Finance`.
- Determine which files, if any, were opened, modified, or copied.
- Review network telemetry for activity following the successful authentication.
- Investigate external IP addresses identified during the investigation using appropriate threat-intelligence sources.

Because `10.10.50.91` is a private internal IP address, public IP reputation services would not establish the reputation of that internal host. The internal asset would instead need to be identified using organizational asset, DHCP, DNS, EDR, or similar telemetry.

## Investigation Limitations

This investigation used a limited synthetic dataset created for training purposes.

The available telemetry did not include:

- Historical account baselines
- Asset ownership information
- Authentication failure reason codes
- Full process command lines
- EDR telemetry
- Detailed file-access events
- Network connection logs
- MFA records
- User validation
- Data-transfer or exfiltration evidence

These limitations prevent a definitive determination of whether the observed activity was authorized or malicious.

The appropriate outcome based on the available evidence was therefore escalation for additional investigation rather than declaring a confirmed compromise.
