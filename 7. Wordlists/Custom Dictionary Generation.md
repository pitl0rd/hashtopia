# Custom Dictionary Generation & Targeted Attack Workflow

_(Hashtopia Style)_

This workflow demonstrates how to **build a context-aware custom dictionary**, enrich it with probabilistic transformations, and apply a **layered attack strategy** that mirrors real-world password construction behavior.

---

## Phase 1 - Target-Aware Dictionary Creation

### Step 1 - Harvest Vocabulary from Target Surface (CeWL)

CeWL extracts human-meaningful terms directly from a website, capturing brand names, product language, slogans, and contextual keywords attackers commonly reuse.

```bash
cewl -d 2 -m 5 -w custom_dict.txt http://www.targetsite.com
```

**Why this matters**

- Users frequently reuse site-specific language in passwords
    
- Depth `-d 2` captures secondary navigation and content pages
    
- Minimum length `-m 5` reduces noise
    

---

## Phase 2 - Linguistic Baseline Expansion

### Step 2 - Merge Common English Vocabulary

Add high-frequency English words to cover generic basewords.

```bash
cat google-1000.txt >> custom_dict.txt
```

**Why this matters**

- Covers fallback passwords when users don’t personalize
    
- Serves as a base for rule-based mutation later
    

Source:  
[https://github.com/first20hours/google-10000-english](https://github.com/first20hours/google-10000-english)

---

## Phase 3 - Probabilistic Password Seeding

### Step 3 - Inject Real-World Password Seeds

Add the **Top 196 real passwords** from Probable Wordlists.

```bash
cat Top196-probable.txt >> custom_dict.txt
```

**Why this matters**

- These represent _observed_ password behavior, not guesses
    
- Excellent anchors for rules, hybrids, and combinators
    

Source:  
[https://github.com/berzerk0/Probable-Wordlists](https://github.com/berzerk0/Probable-Wordlists)

---

## Phase 4 - Structural Expansion via Combination

### Step 4 - Self-Combine High-Value Passwords

Users frequently concatenate familiar terms (e.g., `love123`, `summer2020`, `adminadmin`).

```bash
combinator.bin Top196-probable.txt Top196-probable.txt >> custom_dict.txt
```

**Why this matters**

- Models repetition and paired concepts
    
- Expands search space without brute force
    

---

## Phase 5 - Rule-Driven Mutation

### Step 5 - Apply High-Yield Rules (best64)

Generate mutated candidates using one of the most effective compact rule sets.

```bash
hashcat -a 0 Top196-probable.txt -r best64.rule --stdout >> custom_dict.txt
```

**Why this matters**

- Covers capitalization, digit appends, symbol substitutions
    
- Extremely high ROI per candidate
    

---

## Phase 6 - Dictionary Hygiene (Optional but Recommended)

Before attacking, clean and optimize the dictionary:

```bash
sort custom_dict.txt | uniq > custom_dict.clean.txt
```

Optional filters:

- `len.bin 6 14` → enforce realistic length bounds
    
- `req.bin` → enforce charset policies
    

---

# Target Hash Analysis

## Provided Hash

```
e4821dl6a298092638ddb7cadc26d32f
```

### Initial Observations

|Property|Assessment|
|---|---|
|Length|32 characters|
|Charset|Hexadecimal|
|Likely Type|**MD5**|
|Salted|Unlikely|
|Fast hash|Yes|

> ⚠️ The presence of the letter **`l`** is _not valid hex_.  
> This strongly suggests:

- A transcription error
    
- OR the hash is **not actually MD5**
    

Before attacking, **validate the hash**.

---

## Step 1 - Identify the Hash Type

```bash
hashid e4821dl6a298092638ddb7cadc26d32f
```

or

```bash
hashcat -m 0 --identify e4821dl6a298092638ddb7cadc26d32f
```

**Expected outcome**

- If MD5 → mode `-m 0`
    
- If invalid hex → hash must be corrected first
    

---

# Recommended Attack Strategy

Assuming the hash resolves to **MD5** and is corrected if necessary:

---

## Attack 1 - Custom Dictionary + Rules (Primary)

```bash
hashcat -m 0 -a 0 hash.txt custom_dict.clean.txt -r best64.rule
```

**Why**

- Matches how the dictionary was constructed
    
- Rules amplify coverage without brute force
    

---

## Attack 2 - Hybrid Append (Human Pattern Bias)

Users frequently append years or short numbers.

```bash
hashcat -m 0 -a 6 hash.txt custom_dict.clean.txt ?d?d?d?d
```

Examples generated:

```
password2020
admin1234
welcome1987
```

---

## Attack 3 - Hybrid Prepend (Less Common, Still Valid)

```bash
hashcat -m 0 -a 7 hash.txt ?d?d custom_dict.clean.txt
```

Examples:

```
12password
99summer
```

---

## Attack 4 - Mask-Reduced Brute (Last Resort)

If all above fail, pivot to **behavior-based masks**, not full brute force.

```bash
hashcat -m 0 -a 3 hash.txt ?u?l?l?l?l?d?d
```

Models:

```
Julia84
Admin12
Summer99
```

---

# Why This Attack Is Defensible

This is **not brute force guessing**, it is:

- Linguistically informed
    
- Statistically grounded
    
- Behaviorally realistic
    
- Operationally efficient
    

It mirrors how:

- Real users create passwords
    
- Real attackers prioritize effort
    
- Real assessments should be conducted
[[Wordlists and Dictionaries]]
[[Home]]