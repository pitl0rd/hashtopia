# Full Disk Encryption Processing Examples

These examples demonstrate how **full disk encryption (FDE) credentials** may be processed during **authorized security assessments**, audits, or investigations.  
All actions described here **must only be performed with explicit permission** and within the scope of an approved engagement.

The goal of these workflows is not casual decryption, but to **evaluate the real-world resistance of disk encryption configurations** under controlled conditions.

---

## LUKS (Linux Unified Key Setup)

LUKS protects Linux disks using a passphrase-derived key stored in the disk header.  
Password testing requires extracting this header and treating it as a cracking target.

### Step 1: Extract the LUKS Header

`dd if=<luks_partition> of=luks-header.dd bs=512 count=4097`

This captures the metadata required for offline testing without copying the full disk.

### Step 2: Perform a Dictionary or Targeted Attack

`hashcat -a 0 -m 14600 luks-header.dd dict.txt`

Attack strategy selection depends on passphrase policy, user behavior, and threat model.

---

## TrueCrypt and VeraCrypt

TrueCrypt and VeraCrypt volumes require extracting specific binary regions and treating them as Hashcat-compatible inputs.  
The same extraction approach applies to both tools.

Reference:  
[https://hashcat.net/wiki/doku.php?id=frequently_asked_questions#how_do_i_extract_the_hashes_from_truecrypt_volumes](https://hashcat.net/wiki/doku.php?id=frequently_asked_questions#how_do_i_extract_the_hashes_from_truecrypt_volumes)

---

### Boot Volume

#### Step 1: Extract the Boot Volume Header

`dd if=truecrypt_boot.raw of=truecrypt_boot.dd bs=1 skip=31744 count=512`

#### Step 2: Select the Appropriate Hashcat Mode

`hashcat -a 0 -m xxxx truecrypt_boot.dd dict.txt`

Mode selection depends on encryption and key derivation settings.

---

### Hidden Volume

#### Step 1: Extract Hidden Volume Header

`dd if=truecrypt_hidden.raw of=truecrypt_hidden.dd bs=1 skip=65536 count=512`

#### Step 2: Run Hashcat

`hashcat -a 0 -m xxxx truecrypt_hidden.dd dict.txt`

---

### File-Based Containers

#### Step 1: Extract Initial Header Block

`dd if=truecrypt_file.raw of=truecrypt_file.dd bs=512 count=1`

#### Step 2: Run Hashcat

`hashcat -a 0 -m xxxx truecrypt_file.dd dict.txt`

---

### TrueCrypt / VeraCrypt Mode Reference

(TrueCrypt: 62XY, VeraCrypt: 137XY)

- **PBKDF2 Hash Variants**
    
    - RIPEMD-160
        
    - SHA-512
        
    - Whirlpool
        
    - SHA-256 (VeraCrypt)
        
    - Streebog-512 (VeraCrypt)
        
- **XTS Encryption Modes**
    
    - AES, Serpent, Twofish
        
    - Cascaded combinations
        
    - 512-bit, 1024-bit, and 1536-bit key sizes
        

Correct mode selection is critical for meaningful results.

---

## Windows BitLocker

BitLocker protects Windows volumes using TPM-backed or recovery-password-based encryption.

### Step 1: Image the Encrypted Disk

`sudo dd if=/dev/disk2 of=/path/to/bitlocker_image.dd conv=noerror,sync`

### Step 2: Extract the Hash

`bitlocker2john -i /path/to/bitlocker_image.dd`

### Step 3: Save Output to a Hash File

`hash.txt`

### Step 4: Crack Using John the Ripper

`john --format=bitlocker-opencl --wordlist=dict.txt hash.txt`

#### BitLocker Recovery Password Pattern

Example recovery key:

`236808-089419-192665-495704-618299-073414-538373-542366`

Mask example:

`?d?d?d?d?d?d[-]?d?d?d?d?d?d[-]?d?d?d?d?d?d[-]?d?d?d?d?d?d[-]?d?d?d?d?d?d[-]?d?d?d?d?d?d[-]?d?d?d?d?d?d[-]?d?d?d?d?d?d`
#pitl0rd

---

## Apple FileVault 2 (macOS)

FileVault encrypts macOS disks using CoreStorage or APFS-backed encryption.

### Step 1: Image the Encrypted Disk

`sudo dd if=/dev/disk2 of=/path/to/filevault_image.dd conv=noerror,sync`

### Step 2: Install fvde2john

[https://github.com/kholia/fvde2john](https://github.com/kholia/fvde2john)

### Step 3: Attach the Disk Image

`hdiutil attach -imagekey diskimage-class=CRawDiskimage -nomount /Volumes/path/to/filevault_image.dd`

### Step 4: Extract EncryptedRoot.plist.wipekey

`mmls /Volumes/path/to/filevault_image.dd fls -r -o <offset> /Volumes/path/to/filevault_image.dd | grep -i EncryptedRoot icat -o <offset> image.raw <inode> > EncryptedRoot.plist.wipekey`

### Step 5: Identify CoreStorage Volume

`diskutil list`

### Step 6: Extract the Hash

`sudo fvdeinfo -e EncryptedRoot.plist.wipekey /dev/diskXsY`

### Step 7: Crack the Hash

`john --format=FVDE-opencl --wordlist=dict.txt hash.txt hashcat -a 0 -m 16700 hash.txt dict.txt`

---

## Apple File System (APFS – macOS ≤ 10.13)

### Step 1: Install apfs2john

[https://github.com/kholia/apfs2john](https://github.com/kholia/apfs2john)

### Step 2: Dump Hash Material

`sudo ./bin/apfs-dump-quick /dev/sdX outfile.txt sudo ./bin/apfs-dump-quick image.raw outfile.txt`

**Note:**  
Kholia recommends using `kpartx` for safer disk image handling:  
[https://github.com/kholia/fvde2john](https://github.com/kholia/fvde2john)


[[Processing]]
[[Home]]
#pitl0rd