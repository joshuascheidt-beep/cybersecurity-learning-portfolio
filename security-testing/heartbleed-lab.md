# Heartbleed Vulnerability Lab

## Overview

This lab focused on the Heartbleed vulnerability (CVE-2014-0160), a vulnerability that affected certain versions of OpenSSL.

I completed this lab through TryHackMe to better understand how a flaw in a widely used security protocol can lead to sensitive information being exposed from system memory.

All testing was performed in an authorized TryHackMe training environment.

## Vulnerability

**CVE:** CVE-2014-0160  
**Affected Technology:** OpenSSL  
**Vulnerability Type:** Information Disclosure  
**Component:** TLS/DTLS Heartbeat Extension

Heartbleed resulted from improper bounds checking in vulnerable versions of OpenSSL's implementation of the TLS heartbeat extension.

A malicious heartbeat request could claim that it contained more data than was actually provided. A vulnerable server could then return additional data from its process memory.

This could expose information that was never intended to leave the system.

## What I Practiced

During the lab, I worked through the process of:

- Identifying a system affected by Heartbleed
- Understanding how the TLS heartbeat mechanism works
- Testing the vulnerable service in a controlled environment
- Observing information returned from server memory
- Understanding the security impact of memory disclosure
- Reviewing how the vulnerability can be mitigated

## Security Impact

Heartbleed demonstrated that encrypted communication does not automatically mean the systems handling that communication are secure.

Depending on what was stored in memory at the time of exploitation, an attacker could potentially obtain sensitive information such as:

- Session information
- Authentication data
- User credentials
- Application data
- Cryptographic material

The vulnerability was especially significant because exploitation could occur remotely without requiring authentication.

## Mitigation

Organizations affected by Heartbleed needed to:

1. Update OpenSSL to a patched version.
2. Restart affected services so vulnerable code was no longer running.
3. Replace potentially compromised private keys and certificates.
4. Revoke old certificates where necessary.
5. Reset credentials that may have been exposed.
6. Monitor systems for indications of compromise.

## What I Took Away From the Lab

The biggest takeaway for me was seeing how a relatively small implementation mistake can undermine a security mechanism that organizations depend on.

Heartbleed also reinforced the importance of vulnerability management, patching, certificate management, and understanding what is happening underneath protocols such as TLS.

From a defensive perspective, identifying vulnerable software is only part of the response. Security teams also have to consider what information may have been exposed and what credentials or cryptographic material need to be replaced.

## Skills Demonstrated

- Vulnerability analysis
- CVE research and identification
- TLS/OpenSSL security concepts
- Information disclosure analysis
- Vulnerability mitigation
- Security testing in an authorized lab environment

---

**Lab Platform:** TryHackMe  
**Lab:** Heartbleed  
**Environment:** Authorized cybersecurity training environment
