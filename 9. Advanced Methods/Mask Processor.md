# **Mask Processor (mp64)**

  

**Mask Processor** is a high-performance mask generation engine designed to **constrain and sculpt brute-force keyspaces** using human-realistic rules. It is part of the **hashcat-utils** suite and is most powerful when paired with **Hashcat** via stdin/stdout.

  

Unlike traditional mask attacks that enumerate _all_ combinations, Mask Processor allows you to **remove implausible candidate structures** by limiting repetition, consecutive characters, and character placement.

  

> Think of Mask Processor as _behavior-aware brute force_.

  

Repository:

https://github.com/hashcat/maskprocessor

---

## **Why Mask Processor Exists**

  

Pure mask attacks (?a?a?a?...) are exhaustive but inefficient when:

- Humans rarely repeat the same character many times
    
- Consecutive identical characters are uncommon
    
- Certain character classes cluster in predictable ways
    

  

Mask Processor reduces wasted work by enforcing **structural constraints**, dramatically shrinking keyspace while preserving likely candidates.

---

## **Core Capabilities**

  

Mask Processor extends standard mask notation by adding:

- **Repeat limits** (global character reuse)
    
- **Consecutive limits** (run-length control)
    
- **Custom charsets**
    
- **Streaming output** for pipeline-based attacks
    

  

This makes it ideal for:

- Long-running attacks
    
- Distributed workloads
    
- Slow hashes (bcrypt, scrypt, PBKDF2)
    
- Research into password structure realism
    

---

## **Limiting Repeating Characters**

  

### **Limit maximum identical characters in a password (-r)


Prevents any character from appearing more than _N_ times total.

```
mp64.bin -r 4 ?d?d?d?d?d?d?d?d | hashcat -a 0 -m hash_type hash.txt
```

**Effect:**

- 11112222 - yes
    
- 11111111 - no
    

  

This reflects the reality that most users do not reuse the same digit repeatedly.

---

## **Limiting Consecutive Characters**

  

### **Limit maximum consecutive identical characters ( -q** )

Restricts how many times a character may repeat **in sequence**.

```
mp64.bin -q 4 ?d?d?d?d?d?d?d?d | hashcat -a 0 -m hash_type hash.txt
```

**Effect:**

- 11223344 - yes
    
- 11112 - no


This eliminates unrealistic patterns such as long keyboard holds or accidental repeats.

---

## **Combining Repeat + Consecutive Limits**

### **Enforce both realism constraints together**

```
mp64.bin -r 2 -q 2 ?d?d?d?d?d?d?d?d | hashcat -a 0 -m hash_type hash.txt
```

**Interpretation:**

- No character appears more than **twice total**
    
- No character appears more than **twice consecutively**


This sharply prunes brute-force space while keeping human-plausible candidates.

---

## **Custom Charset with Structural Constraints**

### **Targeted human-style patterns**

```
mp64.bin -r 2 -q 2 -1 aeiuo -2 TGBYHN ?l?2?l?2?d?d?d?d | hashcat -a 0 -m hash_type hash.txt
```

**What this expresses:**

|**Component**|**Meaning**|
|---|---|
|-1 aeiuo|Vowels|
|-2 TGBYHN|Uppercase consonants|
|?l?2?l?2|Alternating lower/upper|
|?d?d?d?d|Numeric suffix|
|-r 2 -q 2|Realistic repetition limits|

This models **name-like words with numeric endings**, one of the most common human password structures.

---

## **Why This Matters**


Mask Processor is not about cracking _faster_, it’s about cracking **smarter**.


It allows you to:

- Remove absurd combinations
    
- Encode behavioral assumptions directly into keyspace
    
- Preserve exhaustiveness _within_ realistic bounds
    
- Make brute force viable against slow hashes

---

## **When to Use Mask Processor**


Use Mask Processor when:

- Full brute force is too large
    
- You understand likely structure but not content
    
- Rules and dictionaries are saturated
    
- You want reproducible, explainable attack logic
 

Avoid it when:

- Passwords are machine-generated
    
- Policy forces randomness
    
- GPU throughput is the only concern

---

## **Key Takeaway**

**Mask Processor turns brute force into a research instrument.**

By constraining repetition and structure, it transforms raw computation into **hypothesis-driven enumeration**, letting you explore the _shape_ of human password space instead of blindly traversing it.


[[Advanced Compositional Attacks]]
[[Home]]

  

#tools
#advanced