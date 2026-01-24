
Per-position Markov models are a statistical way to **model how humans build passwords character-by-character**, rather than guessing whole words or applying fixed rules.

  

Instead of asking _“What words do users choose?”_, Markov attacks ask:

  

> **“Given the characters already typed, what character is most likely to come next?”**

  

This makes Markov techniques especially effective once **dictionary and rule attacks begin to taper off**, but before full brute force becomes necessary.

---

## **Tool Overview**
#tools 

|**Component**|**Purpose**|
|---|---|
|**hcstatgen**|Builds a per-position Markov probability model|
|**statsprocessor (sp64)**|Uses that model to generate password candidates|
|**Hashcat**|Consumes Markov-guided candidates for cracking|

Repository:
https://github.com/hashcat/hashcat-utils 
https://github.com/hashcat/statsprocessor
https://hashcat.net/hashcat/
---

## **HCSTATGEN - Markov Model Generation**

  

### **What It Does**

  

hcstatgen analyzes **known plaintext or cracked passwords** and builds a **per-position probability table** that answers questions like:

- How often does a follow p?
    
- How often does a digit appear at position 7?
    
- Which characters dominate early vs late positions?
    

  

This produces a **.hcstat file**, which is later used to guide guessing.

  

> **Important Behavior**

> hcstatgen always produces a **32MB file**, regardless of input size.

> This is expected and not an error.

---

### **Create a Custom Markov Model**

```
hcstatgen.bin out.hcstat < passwords.txt
```

**Best practices:**

- Use **target-specific cracked passwords**
    
- Separate datasets by region, organization, or application
    
- Avoid mixing radically different password populations
    

  

> Markov models trained on unrelated data will still work, just less efficiently.

---

## **STATSPROCESSOR (sp64)**

  

### **What It Does**

  

statsprocessor is a **high-performance word generator** that emits password candidates based on:

- A Markov model (.hcstat)
    
- Mask-style structural constraints
    

  

It produces guesses in **probability-weighted order**, not lexicographic order.

---

## **Using Markov Models with Hashcat**

  

### **Option 1 - Direct Hashcat Integration**

  

Hashcat can consume Markov models directly during **mask or rule attacks**.

  

#### **Mask + Markov**

```
hashcat -a 3 -m hash_type hash.txt \
  --markov-hcstat=out.hcstat \
  ?a?a?a?a?a?a
```

#### **Dictionary + Rules + Markov**

```
hashcat -a 0 -m hash_type hash.txt dict.txt \
  -r rule.txt \
  --markov-hcstat=out.hcstat
```

**Effect:**

- Reduces wasted guesses
    
- Prioritizes statistically likely character transitions
    

---

### **Option 2 - statsprocessor → Hashcat (stdin)**

  

This approach externalizes guess generation and gives **more control** over length and structure.

```
sp64.bin --pw-min 3 --pw-max 5 out.hcstat ?1?1?1?1?1?1 | \
hashcat -a 0 -m hash_type hash.txt
```

**Advantages:**

- Explicit control over length
    
- Can mix with other generators
    
- Easy to chain with additional tooling
    

---

## **When Markov Attacks Shine**

  

Markov attacks perform best when:

- Dictionaries + rules have plateaued
    
- Passwords are **human-generated but not dictionary-based**
    
- Length and character sets are semi-predictable
    
- Full brute force is still infeasible
    

  

They are especially effective against:

- Legacy password policies
    
- User-chosen passwords with consistent structure
    
- Password reuse patterns
    
Consider an environment with a legacy password policy that requires at least eight characters, one uppercase letter, and one number, but does not enforce randomness or passphrases. After running dictionary attacks with rules, you successfully crack many passwords like Password1, Welcome2023, and Spring!23. Eventually, these approaches plateau: the remaining passwords are no longer simple dictionary words or obvious rule-based variations.

  

At this point, the remaining passwords look more like Tr4v3r89, Bl@ckH0r5e, or M1ch43l77. These passwords are clearly **human-generated**, they contain recognizable letter sequences, substitutions, and repeated structures, but they are not direct dictionary entries, and their variations are too inconsistent for simple rules to capture efficiently.

  

A Markov attack excels in this situation because it does not rely on whole words or predefined rules. Instead, it models **character-to-character transitions** based on previously observed passwords. If users frequently place digits at the end, alternate letters and numbers, or favor certain substitutions (a → @, e → 3), the Markov model learns those tendencies and prioritizes guesses that follow similar patterns. This allows it to reach high-probability candidates much earlier than brute force, while still exploring beyond what dictionaries and rules can express.

In practice, this means Markov attacks often recover passwords that sit in the “middle ground”: too structured to be random, but too irregular to be dictionary-based. This is precisely where large portions of real-world, policy-shaped passwords tend to live.


## **When Markov Is Less Effective**

  

Markov attacks struggle when:

- Passwords are randomly generated
    
- Password managers enforce high entropy
    
- Strong policy + long random strings are used
    

Consider two password datasets drawn from different environments.

In the first environment, users generate passwords using a password manager configured to produce **20-character random strings**, such as:
```text
xF9$LrQ2!aP7KZ@M4wT#
```
These passwords are constructed uniformly at random from a large character set. There are no meaningful character transitions, repeated substrings, or consistent ordering patterns. From the attacker’s perspective, each character is statistically independent of the previous one.


In this case, a Markov model has no useful behavioral signal to exploit. The transition probabilities between characters are effectively flat, so the model cannot prioritize certain sequences over others. As a result, the Markov attack degenerates into **guided brute force**, offering little advantage over traditional exhaustive searching.

By contrast, Markov attacks excel only when real-world structure exists, such as predictable transitions, common prefixes, or repeated patterns, all of which are absent in truly random, high-entropy passwords.

This example highlights a key limitation: **Markov models amplify human behavior, but they cannot create structure where none exists**.


## **Research Takeaways**

- Markov models encode **behavior**, not words
    
- Training data quality matters more than size
    
- Per-target models consistently outperform generic ones
    
- Markov attacks occupy the **middle ground** between rules and brute force
    

  

> If rules describe _how passwords change_,

> Markov describes _how passwords are typed_.

---

## **Recommended Workflow**

1. Run dictionary + rules first
    
2. Generate target-specific .hcstat
    
3. Apply Markov-guided mask attacks
    
4. Escalate to broader brute force only if needed
    

---
[[Advanced Compositional Attacks]]
[[Home]]

  

#tools

#advanced