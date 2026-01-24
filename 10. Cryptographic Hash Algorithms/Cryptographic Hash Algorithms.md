# Cryptographic Hash Algorithms

Cryptographic hash algorithms are one-way mathematical functions that take an input (a password, file, or data structure) and produce a fixed-length output known as a **hash** or **digest**.  
They are designed to be *deterministic*, *irreversible*, and *collision-resistant* (to varying degrees depending on the algorithm).

In the context of password analysis, hash algorithms define:

- how passwords are stored  
- the difficulty of recomputing or brute-forcing values  
- the feasibility of large-scale cracking  
- the security properties of authentication systems  

This page provides a concise overview of the most relevant cryptographic hash algorithms encountered in modern environments.

---

## What Cryptographic Hashes Are *Not*

It is important to distinguish cryptographic hashes from encryption:

- **Hashes are one-way.** They cannot be “decrypted.”  
- **Encryption is two-way.** It requires a key and can be reversed.

Attackers do not decrypt password hashes they **guess candidates and hash them** until they match.

---

# Legacy / Fast Cryptographic Hash Algorithms
These algorithms are extremely fast, making them poor choices for password storage. They are still encountered in legacy systems, breaches, and forensic data.

---

## **MD5**
- **Output:** 128-bit (32 hex chars)  
- **Speed:** Extremely fast  
- **Status:** Broken, collisions trivial  
- **Context:** Old applications, embedded systems  
- **Security impact:** Not suitable for password storage  

---

## **SHA-1**
- **Output:** 160-bit  
- **Speed:** Fast  
- **Status:** Broken (SHAttered)  
- **Context:** Legacy enterprise systems  
- **Security impact:** Not secure for authentication contexts  

---

## **SHA-2 (SHA-224 / SHA-256 / SHA-384 / SHA-512)**
- **Output:** 224–512 bits  
- **Speed:** Fast  
- **Status:** Secure  
- **Context:** TLS, certificates, OS integrity  
- **Security impact:** Still too fast for password hashing  

---

## **NTLM / NT Hash**
- **Output:** 128-bit  
- **Speed:** Extremely fast (no salt)  
- **Context:** Windows authentication  
- **Security impact:** Very weak,  trivial to crack at scale  

---

## **LM Hash**
- **Output:** 128-bit (split into two 7-character halves)  
- **Status:** Fully broken  
- **Context:** Legacy Windows  
- **Security impact:** Instantly recoverable  

---

# Modern / Slow Password Hashing Algorithms
These incorporate salting, iteration counts, and memory hardness to resist brute-force attacks.

---

## **bcrypt**
- **Output:** 60-character format  
- **Includes:** Salt + cost factor  
- **Strength:** Slow, adaptive  
- **Context:** Web apps, Linux systems  
- **Security impact:** Strong when cost is configured correctly  

---

## **PBKDF2 (Password-Based Key Derivation Function 2)**
- **Output:** Variable  
- **Includes:** Salt + iterations  
- **Strength:** Widely adopted, standardized  
- **Weakness:** Not memory-hard  
- **Context:** Active Directory, password managers  
- **Security impact:** Strong with high iteration counts  

---

## **scrypt**
- **Output:** Variable  
- **Strength:** Memory-hard; GPU resistant  
- **Weakness:** High resource usage  
- **Context:** Some authentication schemes, cryptocurrency  
- **Security impact:** Very strong when configured properly  

---

## **Argon2 (Argon2i, Argon2d, Argon2id)**
- **Output:** Variable  
- **Strength:** Memory-hard, modern best practice  
- **Context:** Contemporary security frameworks  
- **Security impact:** Top-tier password hashing algorithm  

---

# File Integrity / Application Hashes
These are secure for integrity checking but are **not designed for password hashing** because they are too fast.

---

## **SHA-3 Family (Keccak)**
- **Output:** SHA3-224, SHA3-256, SHA3-384, SHA3-512  
- **Strength:** Strong sponge construction  
- **Context:** Integrity, cryptographic systems  
- **Security impact:** Not used for password hashing  

---

## **RIPEMD-160**
- **Output:** 160-bit  
- **Context:** Legacy crypto systems, blockchain  
- **Security impact:** Secure but fast,  not recommended for passwords  

---

## **Whirlpool**
- **Output:** 512-bit  
- **Design:** AES-like block cipher components applied to a hash function  
- **Strength:** Cryptographically strong; no practical attacks  
- **Context:** File integrity, cryptographic applications, some Linux tools  
- **Security impact:** Very strong hash for integrity, but **too fast** to be used for password hashing  
- **Notes:** Often encountered in cryptographic toolkits; rarely used for authentication systems  

---

# Salting and Hash Format Behavior

Modern password hashing functions include:

- **Salt** - prevents precomputed (rainbow table) attacks  
- **Cost factor** - slows down brute-force attempts  
- **Memory hardness** - greatly reduces GPU/ASIC advantages (Argon2, scrypt)  

Key principle:

- **Fast hashes = easy to crack**  
- **Slow hashes = intentionally difficult to crack**

This distinction is critical when evaluating real-world password risk.

---

# Hash Algorithm Quick Reference Table

| Algorithm | Type | Speed | Salted? | Modern Status |
|----------|------|--------|---------|----------------|
| MD5 | Fast hash | Very fast | ❌ | Broken |
| SHA-1 | Fast hash | Fast | ❌ | Broken |
| SHA-2 | Fast hash | Fast | ❌ | Secure, but too fast |
| NTLM | Auth hash | Extremely fast | ❌ | Very weak |
| LM | Auth hash | Very fast | ❌ | Broken |
| bcrypt | Password hash | Slow | ✔ | Strong |
| PBKDF2 | KDF | Slow | ✔ | Strong (config-dependent) |
| scrypt | Memory-hard | Slow + RAM | ✔ | Strong |
| Argon2 | Memory-hard | Slow + RAM | ✔ | Best practice |
| SHA-3 | Fast hash | Fast | ❌ | Not suitable for passwords |
| RIPEMD-160 | Fast hash | Fast | ❌ | Legacy |
| Whirlpool | Fast hash | Fast | ❌ | Strong but too fast for passwords |

---

# Intended Outcome

Readers should understand:

- Which hash algorithms are fast vs. slow  
- Why legacy hashes (MD5, SHA-1, NTLM) are insecure  
- Why modern password hashing uses salts, cost factors, and memory hardness  
- Where algorithms like Whirlpool and SHA-3 fit into analysis workflows  
- How hash choice impacts real-world password security  

This page forms the foundation for all hash-related processing and analysis within Hashtopia.

[[Home]]

#research #education 


