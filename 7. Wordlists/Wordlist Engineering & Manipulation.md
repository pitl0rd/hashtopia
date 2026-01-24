


Wordlists are not static artifacts they are _constructed_, _reduced_, and _reshaped_ to match observed human password behavior.  
This page covers common utilities used to **generate**, **normalize**, **combine**, and **filter** wordlists for downstream use in cracking, analysis, and pattern research.

It is worth mentioning here, and everywhere else that I can on the site, but its important to know that: If you are new or doing this for the first time, you are probably going to make mistakes here. **Double check your process and understand what you are trying to create** before you hit the 'generate' button. Learning the hard way here results in protracted generation cycles that **will eat up large chunks of your storage**, probably in TeraBytes.

---

## John the Ripper - Wordlist Generation

John can be used as a **candidate generator**, not just a cracker.  
Using `--stdout`, John emits transformed candidates without attacking hashes.

### Generate candidates that meet complexity filters

```bash
john --wordlist=dict.txt --stdout --external:[filter_name] > outfile.txt
```

**Use case**

- Enforce enterprise-specific password policies
    
- Pre-generate compliant candidates for Hashcat
    
- Research how policy constraints affect entropy
    

---

## Stemming (Base-Word Extraction)

**Stemming** strips passwords down to their linguistic or structural core (“base word”).  
This is useful for:

- Discovering semantic reuse
    
- Building smarter combinator dictionaries
    
- Separating _meaning_ from _mutation_
    

### Extract lowercase alphabetic strings

```bash
sed 's/[^a-z]//g' passwords.txt > lowercase.txt
```

### Extract uppercase alphabetic strings

```bash
sed 's/[^A-Z]//g' passwords.txt > uppercase.txt
```

### Extract all alphabetic strings (any case)

```bash
sed 's/[^a-zA-Z]//g' passwords.txt > alpha.txt
```

### Extract digits only

```bash
sed 's/[^0-9]//g' passwords.txt > digits.txt
```

**Research value**

- Identify base words reused across multiple mutations
    
- Quantify how often numbers vs words carry meaning
    
- Feed cleaned bases into combinator or hybrid attacks
    

---

## Combinator

Combines multiple dictionaries by appending each word to every other word.

```bash
combinator.bin dict1.txt dict2.txt > combined.txt
combinator3.bin dict1.txt dict2.txt dict3.txt > combined.txt
```

**Use case**

- FirstName + Year
    
- Word + Suffix
    
- OrganizationName + Role
    

**Strength**

- Explodes _semantic combinations_ efficiently
    
- Often outperforms brute force when meaning is present
    

---

## CUTB - Substring Extraction

Cuts fixed-length substrings from dictionary entries.

```bash
cutb.bin offset length < infile.txt > outfile.txt
```

### Example: extract first 4 characters

```bash
cutb.bin 0 4 < dict.txt > stems.txt
```

**Use case**

- Extract prefixes
    
- Study truncation habits
    
- Generate short base words from long candidates
    

---

## RLI - De-duplication

Removes duplicates by comparing one file against others.

```bash
rli dict1.txt outfile.txt dict2.txt
```

**Use case**

- Prevent redundant candidates
    
- Clean merged dictionaries
    
- Reduce attack keyspace without losing coverage
    

---

## REQ - Character Class Filtering

Filters candidates by required character classes.

|Flag|Class|
|--:|---|
|1|Lowercase|
|2|Uppercase|
|4|Digits|
|8|Special|

### Example: lowercase + uppercase only (1 + 2 = 3)

```bash
req.bin 3 < dict.txt
```

**Use case**

- Enforce policy compliance
    
- Separate human-chosen passwords from generated noise
    

---

## COMBIPOW

Generates _unique combinations_ of a dictionary  
⚠️ Dictionary must be **≤ 64 lines**

```bash
combipow.bin dict.txt
combipow.bin -1 dict.txt
```

**Use case**

- Small, high-signal keyword sets
    
- Passwords built from multiple short tokens
    

---

## EXPANDER

Splits dictionary entries into individual characters (up to 4-char chains).

```bash
expander.bin < dict.txt
```

**Use case**

- Character-level frequency analysis
    
- Rule generation
    
- Micro-pattern research
    

---

## LEN - Length Filtering

Filters candidates by length.

```bash
len.bin <min> <max> < dict.txt
```

### Example: allow 5–10 characters

```bash
len.bin 5 10 < dict.txt
```

**Use case**

- Align wordlists to observed length distributions
    
- Reduce waste in brute-force-adjacent attacks
    

---

## MORPH

Automatically generates insertion rules based on frequent character chains.

```bash
morph.bin dict.txt depth width pos_min pos_max
```

**Use case**

- Discover common mutation patterns
    
- Generate targeted rule sets from real data
    

---

## PERMUTE

Generates permutations using the _Countdown QuickPerm Algorithm_.

```bash
permute.bin < dict.txt
```

**Use case**

- Passwords built from rearranged tokens
    
- Low-entropy but non-obvious constructions
    

---

## CRUNCH - Wordlist Generator

Generates all combinations from a defined charset.

```bash
crunch <min> <max> <charset> -o outfile.txt
```

### Example: 8-char hex strings

```bash
crunch 8 8 0123456789ABCDEF -o hex8.txt
```

**Use case**

- PINs, hex tokens, constrained formats
    
- Research into purely synthetic password spaces
    

---

## Key Takeaway (Hashtopia Principle)

> **Wordlists are hypotheses.**  
> The better your hypothesis about human behavior, the smaller, and more effective, your candidate space becomes.

This tooling exists to **shape probability**, not to exhaust possibility :)

---
#sudad #wordlists 
[[Wordlists and Dictionaries]]
[[Home]]