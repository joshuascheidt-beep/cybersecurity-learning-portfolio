# Cybersecurity Learning Portfolio

## About Me

I am an active-duty U.S. Marine with 17 years of experience in technical leadership, secure communications, network operations, training, and mission-critical systems.

I am building on that background through hands-on cybersecurity training focused on security operations, network analysis, Windows security, and digital forensics. This portfolio documents not only the tools I am learning, but also how I analyze technical evidence, interpret results, and communicate my findings.

## Featured Hands-On Labs

These projects document hands-on cybersecurity investigations completed in authorized training environments. Each project includes technical observations, analysis, and supporting evidence.

| **Lab** | **What I Documented** | **View Project** |
| --- | --- | --- |
| Wireshark: The Basics | TCP handshake analysis, packet inspection, protocol analysis, and completion evidence | [Wireshark Lab](network-analysis/wireshark-basics.md) |
| Nmap: The Basics | Host/service discovery, service-version detection, six observed open TCP ports, and findings analysis | [Nmap Lab](network-scanning/nmap-basics.md) |
| Tcpdump: The Basics | Five-packet TCP capture, command and flag analysis, TCP flags, and acknowledgment behavior | [tcpdump Lab](network-analysis/tcpdump-basics.md) |
| Windows Security Fundamentals | PowerShell account enumeration, administrator review, Security Event Log analysis, and event correlation | [Windows Security Lab](windows-linux/windows-security-lab.md) |
| **SOC Investigation — Windows Authentication** | Investigated repeated Windows authentication failures and privileged account activity using PowerShell. Correlated authentication and endpoint telemetry to reconstruct a timeline, analyze post-authentication behavior, and determine whether escalation was warranted. | [View Investigation](soc-investigations/windows-authentication-investigation.md) |
| Heartbleed (CVE-2014-0160) | Analyzed the OpenSSL TLS heartbeat vulnerability, memory disclosure impact, and defensive remediation | [Heartbleed Lab](security-testing/heartbleed-lab.md) |

> **Lab Scope:** These projects were completed in authorized training environments. Findings are based on documented exercises and do not represent professional incident-response engagements or third-party security assessments.

## Technical Skills Demonstrated

**Network Analysis**
- Wireshark
- tcpdump
- TCP/IP traffic analysis
- Packet filtering and inspection
- TCP connection analysis

**Network Discovery**
- Nmap
- Port scanning
- Service and version detection
- Network reconnaissance in authorized environments

**Windows Security**
- PowerShell
- Local user and group enumeration
- Administrator membership review
- Windows Security Event Log analysis
- Event filtering and correlation
- Least-privilege analysis

**Security Fundamentals**
- Public-key cryptography
- Hashing
- Password-security concepts
- John the Ripper in authorized training environments

## Featured Investigation: Windows Event Correlation

During my Windows Security lab, I used PowerShell to investigate Windows Security Event Log activity and correlate object-access events.

I identified and analyzed Events 4656 and 4658 and correlated records using multiple data points, including:

- Timestamp
- Handle ID
- Process ID
- Process information
- Security principal

The exercise reinforced an important investigative principle: an Event ID alone does not establish malicious activity. Security events need to be evaluated in context and correlated with supporting evidence before reaching a conclusion.

[View the Windows Security investigation](windows-linux/windows-security-lab.md)

## Training and Education

- **Google Cybersecurity Professional Certificate** — Completed
- **CompTIA Security+** — In preparation
- **B.S. Cybersecurity, Digital Forensics Concentration — American Military University** — Planned

## Career Focus

I am developing toward civilian cybersecurity roles where I can combine technical investigation with the leadership and operational experience developed throughout my Marine Corps career.

Areas of interest include:

- Security Operations Center (SOC) Analysis
- Cybersecurity Analysis
- Network Security
- Digital Forensics

## Portfolio Sections

- [Network Analysis](network-analysis/)
- [Network Scanning](network-scanning/)
- [Windows and Linux](windows-linux/)
- [Cryptography](cryptography/)
- [Security Testing](security-testing/)

## Professional Background

My Marine Corps experience includes technical leadership, secure communications, network operations, personnel development, training, and accountability for mission-critical technical equipment.

The cybersecurity projects in this repository represent independent hands-on training rather than professional cybersecurity employment. My goal is to demonstrate how I am applying an established technical and operational background to cybersecurity analysis.

## Current Development

I am continuing to expand this portfolio through hands-on labs involving:

- Windows and Linux security
- Network traffic analysis
- Security monitoring and investigation
- PowerShell
- Security+ concepts
- Digital forensics
- Defensive security workflows

As my technical capabilities develop, additional investigations and projects will be added with supporting evidence and documented analysis.

## Contact

**LinkedIn:** [Joshua Scheidt](https://www.linkedin.com/in/joshua-scheidt-714561328)

