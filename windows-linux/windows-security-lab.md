# Windows Security Fundamentals — Hands-On Lab

## Objective

Practice Windows security administration and investigation using PowerShell, with a focus on local accounts, group membership, and the principle of least privilege.

## Lab Environment

- Operating system: To be confirmed
- Environment: Authorized training system
- Tool: PowerShell

## Commands Executed

```powershell
Get-LocalGroup
Get-LocalUser | Select-Object Name, Enabled, Description
Get-LocalGroupMember -Group "Administrators"
```

## Observed Results

- Identified six local user accounts: three enabled and three disabled.
- Observed that the built-in Administrator account was enabled and the built-in Guest account was disabled.
- Identified two local user accounts in the Administrators group.
- Enumerated local security groups, including Event Log Readers and Remote Desktop Users. Group membership was not inspected for those groups.

## Security Analysis

Local account enumeration provides visibility into account status and potential administrative access. Reviewing the Administrators group helps identify accounts with elevated privileges and supports least-privilege assessments.

An enabled Administrator account or multiple administrator memberships is not automatically a security finding. Determining whether these settings are appropriate requires additional context, such as organizational policy and the intended purpose of each account.

**Scope:** Read-only inspection of an authorized TryHackMe Windows training machine. No account permissions or settings were changed.

## Evidence

_Sanitized screenshots will be added after the exercise._

## Scope and Limitations

This project documents an authorized learning exercise. Account information and other sensitive details will be excluded from the public repository.
