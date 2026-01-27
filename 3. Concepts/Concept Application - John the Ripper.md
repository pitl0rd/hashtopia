This guide is intended as an introduction to a **general password cracking workflow** using **John the Ripper (JtR)**, presented for educational and defensive purposes.

We assume that you are comfortable working from the command line and that you are analyzing **NTLM password hashes**. In this scenario, the hashes represent user-selected plaintext passwords that have been transformed using a fast hashing algorithm.

NTLM is classified as a **fast hash**, meaning it can be computed with very low computational cost. From an analytical perspective, this allows:

- A large number of password guesses per second  
- Broad exploration of likely password structures  
- Efficient demonstration of cracking methodologies  

Because the underlying passwords are typically words, phrases, or predictable variations, **wordlist-based attacks** are an appropriate starting point. If the data instead reflected high-entropy secrets or machine-generated values, a different strategy would be required.

Although this guide uses **John the Ripper**, the overall methodology reflects a **tool-agnostic approach**. The key principle is to observe results at each stage and continuously refine your strategy based on what the data reveals.

---

## Preparing Hashes for John the Ripper

Before beginning, ensure your NTLM hashes are in a format John understands. Guidance on normalization and preparation can be found in:

- **[[Windows Local Password Hashes]]**
- **[[Windows Domain Password Hashes]]**
- 

For learning or benchmarking, curated datasets are available in  
**[[Example Hashes]]**.

---

## Custom Wordlist Attack  
#wordlists #john_attacks

Begin with the most targeted data available: a **custom wordlist** derived from known passwords, organizational context, naming conventions, or previously cracked values.

This captures environment-specific behavior and should always be tested first.

```bash
john --format=NT --wordlist=custom_list.txt hashes.txt
````

This stage identifies exact matches and establishes a baseline understanding of password quality.

---

## **Custom Wordlist Attack + Rules**

  

#wordlists #john_attacks

  

After testing exact matches, expand coverage using **rules** to generate common variations such as appended digits, capitalization changes, and simple substitutions.

  

John’s built-in [[Rules Analysis|rulesets]] are well-curated for real-world behavior.

```
john --format=NT --wordlist=custom_list.txt --rules hashes.txt
```

Rules allow John to prioritize likely human transformations instead of guessing blindly.

---

## **Broad Wordlist Attacks**

  

#wordlists #john_attacks

  

Once custom data is exhausted, expand outward to **public wordlists** and breach-derived dictionaries.

```
john --format=NT --wordlist=dict.txt hashes.txt
```

This step often recovers widely reused passwords and common defaults.

---

## **Broad Wordlist Attacks + Rules**

  

#wordlists #john_attacks

  

Combine public wordlists with rules to capture **modified versions of common passwords**, such as seasonal variants or policy-driven changes.

```
john --format=NT --wordlist=dict.txt --rules hashes.txt
```

At this stage, John is modeling typical user adaptation rather than raw guessing.

---

## **Fingerprinting and Feedback Loop**

  

#wordlists #john_attacks

  

As passwords are cracked, John automatically stores them in its john.pot file. These results represent **empirical evidence of user behavior**.

  

Extract newly cracked passwords and add them back into your custom wordlist to refine future attacks.

```
john --show hashes.txt | cut -d':' -f2 >> custom_list.txt
```

Re-run targeted rule-based attacks using the enriched list.

---

## **Mask Attacks**

  

#masks #john_attacks

  

Once dictionary-based methods plateau, shift to **mask attacks** to explore common structural patterns.

  

Masks allow you to specify likely password formats without assuming specific words.

```
john --format=NT --mask='?u?l?l?l?d?d' hashes.txt
```

This targets patterns such as a capitalized word followed by digits; structures frequently observed in real datasets.

---

## **Hybrid Wordlist + Mask Attacks**

  

#hybrid #john_attacks

  

Hybrid attacks combine real words with structural assumptions, modeling passwords like:

- `Company2024
    
- `Welcome!23
    

```
john --format=NT --wordlist=dict.txt --mask='?d?d' hashes.txt
```

This approach bridges dictionary knowledge with pattern awareness.

---

## **Combinator-Style Attacks**

  

#combinator #john_attacks

  

To model multi-part passwords, John can apply rules or incremental strategies that effectively combine elements.

  

For example, enabling aggressive rules can surface joined words and repeated structures:

```
john --format=NT --wordlist=dict.txt --rules=All hashes.txt
```

This approximates combinator behavior without explicit pairing files.

---

## **Incremental (Smart Brute Force)**
#bruteforce #john_attacks

As a final stage, John’s **incremental mode** performs a guided brute-force attack informed by character frequency and order.

```
john --format=NT --incremental hashes.txt
```

Incremental mode is not purely random; it prioritizes more likely character sequences first.

  

This step should be used selectively, as the keyspace grows exponentially with password length.

---

## **Interpreting Results**

  

Throughout this workflow, the goal is not merely to crack passwords, but to **learn from them**:

- Which patterns appear first?
    
- Where does cracking slow down?
    
- Which structures resist guessing longest?
    

  

These observations inform both **defensive improvements** and **future analytical strategies**.

---

## **Key Takeaway**

  

John the Ripper is about **ordering guesses intelligently**, adapting to evidence, and focusing effort where human behavior concentrates probability. This workflow emphasizes reasoning over raw computation, aligning with Hashtopia’s focus on real-world risk and empirical [[Password Pattern Analysis]].

---
#pitl0rd
[[1. Concepts|Concepts]]
[[Home]]
