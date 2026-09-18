# Nmap: The Basics — TryHackMe

## Overview

Completed the TryHackMe *Nmap: The Basics* room as part of my hands-on cybersecurity training. The room introduces network reconnaissance using Nmap, including discovering live hosts, identifying open ports, and detecting service versions.

## Topics Covered

- Host discovery and identifying reachable systems.
- Port scanning and interpreting open-port results.
- Service and version detection.
- Understanding how network visibility supports security assessments.

## Security Relevance

Nmap helps security teams understand which hosts and services are exposed on a network. This information can support asset discovery, authorized vulnerability assessments, and investigation of unexpected network services.
## Hands-On Command Analysis

### Command 1: Service and Version Detection

## Hands-On Command Analysis

### Service and Version Detection

**Lab environment:** TryHackMe — Nmap: The Basics

**Command executed:**

```bash
nmap -sV 10.66.190.79
```
**Objective:** Identify open TCP ports on the assigned lab target and attempt to determine the services and software versions running on them.

### Observed Results

| Port | State | Service | Version / Notes |
|---|---|---|---|
| 7/tcp | Open | echo | Echo service identified |
| 9/tcp | Open | tcpwrapped | Underlying service not identified |
| 13/tcp | Open | daytime? | Date/time response; identification uncertain |
| 17/tcp | Open | qotd? | Quotation responses; identification uncertain |
| 22/tcp | Open | ssh | OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 |
| 8008/tcp | Open | http | lighttpd 1.4.74 |

**Scan summary:** One host was up. Nmap reported six open TCP ports and completed the scan in 13.49 seconds.

### Analysis

- Used `-sV` to perform service and version detection against an authorized TryHackMe lab target.
- Identified SSH on TCP port 22 and HTTP on TCP port 8008, including reported software versions.
- Observed that Nmap marked the services on ports 13 and 17 with question marks and generated fingerprints because it could not confidently identify them.
- Learned that service identification is not always definitive and that open ports and version information alone do not establish a vulnerability.

**Scope:** This scan was conducted against a TryHackMe-assigned training machine. No exploitation or vulnerability confirmation was performed as part of this documented exercise.


**Purpose:** Scan a host to identify open ports and attempt to determine the services and software versions running on them.

**Flag explanation:**
- `nmap`: Launches the network scanning tool.
- `-sV`: Enables service and version detection.
- `<LAB_TARGET_IP>`: Placeholder for an authorized TryHackMe target address.

**My lab results:**

_To be completed after running the command against an authorized lab target._

**What I learned:**

_To be completed after reviewing the actual scan output._

## Completion Evidence

- [View my TryHackMe Nmap completion achievement](https://tryhackme.com/room/nmap?utm_campaign=social_share&utm_medium=social&utm_content=share-completed-room&utm_source=copy&sharerId=6a2ce4065b8629fb7af768d6)
- [My public TryHackMe profile](https://tryhackme.com/p/joshua.scheidt)
- ![TryHackMe Nmap completion screenshot](../nmap-completion.png)

*This entry documents a guided training room, not a professional penetration test or an assessment of a third-party network.*
