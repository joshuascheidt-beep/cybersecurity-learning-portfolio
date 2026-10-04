# Security Testing & Password Auditing

## Overview
This section documents my hands-on security testing training through TryHackMe and independent practice.

My focus is understanding how security testing can help organizations identify weaknesses, evaluate password security, and improve defensive controls.

All exercises are conducted in authorized training environments using lab-provided or intentionally created test data.

## Tools & Technologies
- John the Ripper
- Linux command-line interface
- Password hashes and hashing concepts
- OpenSSL and TLS security concepts
- Vulnerability analysis and CVE research
  
## Completed TryHackMe Training

### John the Ripper: The Basics
Studied how John the Ripper is used for password auditing in controlled environments.

### Heartbleed (CVE-2014-0160)

Investigated the Heartbleed vulnerability in an authorized TryHackMe environment and examined how improper bounds checking in OpenSSL's TLS heartbeat implementation can expose process memory.

Topics covered:
- Understanding CVE-2014-0160 and the TLS heartbeat extension
- Identifying vulnerable OpenSSL implementations
- Observing information disclosure from server memory
- Evaluating the potential exposure of credentials, session data, and cryptographic material
- Understanding patching, certificate replacement, and credential rotation as remediation steps

**[View the full Heartbleed lab write-up](heartbleed-lab.md)**

Topics covered:
- Understanding password hashes
- Identifying common hash formats
- Using John the Ripper in an authorized lab
- Understanding the role of wordlists in password auditing
- Recognizing the importance of strong passwords and secure password storage

## Cybersecurity Applications
Password auditing and security testing can help security teams:
- Identify weak passwords in authorized assessments
- Evaluate password security policies
- Understand password-related attack techniques
- Recommend stronger authentication practices
- Support security awareness and risk reduction

## Lab Documentation
Detailed write-ups will be added as I document my completed exercises.

Write-ups will include:
- Lab objectives
- Tools and commands used
- Testing methodology
- Sanitized observations
- Lessons learned
- Defensive security relevance

No real credentials, sensitive information, or unauthorized testing results will be published.

## Next Steps
- Document my John the Ripper training
- Practice password auditing with intentionally created test hashes
- Explore password policy assessment
- Continue learning ethical penetration testing and defensive security techniques
