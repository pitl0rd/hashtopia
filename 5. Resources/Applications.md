# Hash Analysis & Cracking Tool Ecosystem

This page provides an overview of **applications commonly used during authorized password security analysis**, including local cracking tools, distributed cracking platforms, and online services.

All tools referenced here must be used **only** within the scope of approved security assessments, audits, research environments, or educational labs.

---

## Hash Suite

### Overview

**Hash Suite** is a Windows-based password hash analysis and cracking application focused primarily on **Windows authentication hashes**.

Instead of attempting to reverse hashes directly, Hash Suite follows the same process used by authentication systems:

1. Generate candidate passwords
    
2. Hash each candidate using the same algorithm
    
3. Compare results against stored hashes
    

This approach is effective because:

- Human-chosen passwords are rarely random
    
- Many password hash functions (especially legacy Windows hashes) are fast to compute
    
- Users tend to reuse common patterns and transformations
    

---

### Hashing Background (Windows Context)

Windows does **not** store plaintext passwords.  
Instead, it stores cryptographic hash values derived from user passwords.

Key properties:

- Hash functions are **one-way**
    
- Passwords cannot be mathematically “reversed”
    
- Validation occurs by hashing the supplied password and comparing results
    

As a result, password recovery relies on **guessing and comparison**, not decryption.

---

### Key Providers in Hash Suite

Hash Suite generates candidate passwords using multiple **key-providers**, each optimized for different password behaviors.

#### 1. Charset (Brute Force)

- Tries all combinations of a defined character set
    
- High computational cost
    
- Most effective against short or highly constrained passwords
    

#### 2. Wordlist

- Uses predefined dictionaries
    
- Extremely effective due to real-world password reuse
    
- Low resource cost compared to brute force
    

#### 3. Keyboard

- Generates passwords based on adjacent keyboard keys
    
- Models common typing patterns (e.g., `qwerty`, `asdf`)
    

#### 4. Phrases

- Combines multiple dictionary words into longer phrases
    
- Targets passphrases and policy-driven length requirements
    

#### 5. DB Info

- Reuses usernames and previously cracked passwords
    
- Effective when combined with transformation rules
    
- Exploits organizational naming and reuse patterns
    

#### 6. LM2NT

- Automatically converts cracked LM hashes into NTLM equivalents
    
- Exploits the structural weakness of legacy LM hashing
    

---

### Rules Engine

Rules apply **predictable transformations** to base words, such as:

- Capitalization
    
- Appending numbers or symbols
    
- Substitutions (`a → @`, `s → $`)
    
- Incrementing digits
    

Rules dramatically increase effectiveness by modeling **human password modification behavior** rather than random guessing.

---

## HashView

### Overview

**HashView** is a web-based dashboard designed to provide **visual insight into cracking activity**.

Key characteristics:

- Uses its **own internal database**
    
- Can consume significant storage over time
    
- Intended as a quality-of-life visualization layer
    
- Typically run as a persistent server/agent pair
    

Operational notes:

- Should be launched within a `screen` or equivalent session
    
- Requires manual restart after reboot
    
- Best suited for ongoing analysis environments
    

---

## Distributed Cracking Platforms

Distributed cracking systems allow workloads to be split across multiple hosts or GPUs, significantly reducing time-to-result.

### Common Platforms

- **Hashtopolis**  
    [https://github.com/s3inlc/hashtopolis](https://github.com/s3inlc/hashtopolis)  
    Enterprise-grade distributed cracking management
    
- **Hashstack**  
    [https://sagitta.pw/software/](https://sagitta.pw/software/)  
    Lightweight distributed hash cracking
    
- **DistHC**  
    [https://github.com/unix-ninja/disthc](https://github.com/unix-ninja/disthc)  
    Distributed Hashcat controller
    
- **CrackLord**  
    [http://jmmcatee.github.io/cracklord/](http://jmmcatee.github.io/cracklord/)  
    Centralized cracking orchestration
    
- **Hashtopus**  
    [http://hashtopus.org](http://hashtopus.org)  
    Web-based Hashcat distribution platform
    
- **HashView**  
    [http://www.hashview.io](http://www.hashview.io)  
    Web visualization layer
    
- **Clortho**  
    [https://github.com/ccdes/clortho](https://github.com/ccdes/clortho)  
    Distributed cracking framework
    

---

## Online Hash Cracking Services

Online services provide **convenience and speed** for common hash types but introduce trade-offs related to:

- Data exposure
    
- Limited transparency
    
- Unknown datasets or techniques
    

These services are typically used for:

- Validation
    
- Research comparison
    
- Known weak hash types
    

### Notable Services

- **CMD5**  
    [https://www.cmd5.org](https://www.cmd5.org)
    
- **Crack.sh**  
    [https://crack.sh](https://crack.sh)  
    Known for fast DES cracking
    
- **GPUHash**  
    [https://gpuhash.me](https://gpuhash.me)
    
- **CrackStation**  
    [https://crackstation.net](https://crackstation.net)
    
- **OnlineHashCrack**  
    [https://www.onlinehashcrack.com](https://www.onlinehashcrack.com)
    
- **HashHunters**  
    [http://www.hashhunters.net](http://www.hashhunters.net)
    
- **Hash.Help**  
    [https://hash.help](https://hash.help)
    

---

## Analytical Perspective

No single tool provides full coverage.

Effective password analysis typically combines:

- Local cracking tools (control and transparency)
    
- Distributed platforms (scale and speed)
    
- Visualization dashboards (insight and reporting)
    
- External services (benchmarking and validation)
    

The value lies not in **recovering passwords**, but in:

- Understanding failure modes
    
- Measuring resistance
    
- Identifying systemic weaknesses
    
- Informing policy and defensive design

[[Resources]]
[[Home]]