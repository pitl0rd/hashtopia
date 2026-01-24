
This section provides two complementary tools:

1. **A Rule-Selection Decision Tree** – _What should I try next, and why?_
    
2. **Rule vs Mask vs Hybrid Comparison** – _Which attack class fits the evidence I have?_
    

These are intended to be used **together** during active password analysis and in conjunction with the Hashtopia [[2. Cracking Methodology|mehtodology]].

---

## 1. Rule-Selection Decision Tree

Use this decision tree **after you have plaintext samples or Pipal-style statistics**.

### Step 1 - Do You Have Any Cracked Passwords?

- **No**
    
    - Start with **[[Masks]]** (policy-driven) or **[[Rule Selection & Attack Strategy Guide|Light Rules]] (best64 / nsa64rule)**
        
- **Yes**
    
    - Proceed to Step 2
        

---

### Step 2-  Do Cracked Passwords Share a Base Word?

Examples:

- `password1`, `password!`, `Password2023`
    
- `Summer21`, `Summer22!`
    
- **Yes → Base word reuse detected**
    
    - Use:
        
        - **[[Rules Analysis]]** (`best64`, `dive`, `OneRule`)
            
        - **Hybrid attacks** (Dict + digits / years)
            
- **No**
    
    - Proceed to Step 3
        

---

### Step 3 - Do You See Structural Consistency?

Examples:

- Capitalized word +  digits
    
- Word + year
    
- Repeated suffixes (`!`, `@`, `123`)
    
- **Yes → Structure known**
    
    - Use:
        
        - **Masks** if structure is strict
            
        - **Rules** if structure varies slightly
            
- **No**
    
    - Proceed to Step 4
        

---

### Step 4- Are Variations Minor or Extensive?

#### Minor Variations

Examples:

- Case changes
    
- Single digit or symbol changes
    
- Simple leetspeak
    

→ **Rules**

- `best64.rule`
    
- `nsa64.rule`
    
- `hob064.rule`
    

#### Extensive / Unknown Variations

Examples:

- Multiple transformations
    
- Unclear ordering
    
- Long passwords
    

→ **Hybrid**

- Dict + Mask
    
- Dict + Rules
    
- Rules + Mask (advanced)
    

---

### Step 5 - Time or Resource Constrained?

- **Yes**
    
    - Use:
        
        - `best64`
            
        - `nsa64rule`
            
        - Small custom rule set
            
- **No**
    
    - Escalate to:
        
        - `dive.rule`
            
        - `generated.rule`
            
        - Random rules (`-g`)
            

---

## 2. Rule vs Mask vs Hybrid Comparison

|Dimension|Rules|Masks|Hybrid|
|---|---|---|---|
|**Purpose**|Modify known words|Enforce structure|Combine words + structure|
|**Best When**|You have base words|You know format|You know _both_|
|**Human Bias Modeled**|High|Medium|Very High|
|**Keyspace Control**|Indirect|Precise|Semi-precise|
|**Performance**|Excellent|Excellent|Good|
|**Setup Complexity**|Low–Medium|Medium|Medium–High|
|**Adaptability**|Very high|Low|High|
|**Policy Awareness**|Weak|Strong|Strong|
|**Requires Plaintext?**|Yes (ideally)|No|Optional|
|**Example Tools**|best64, dive|hcmask|Dict + ?d?d|

---

### When to Prefer Each

#### Use **Rules** When:

- You have **cracked passwords**
    
- Users reuse base words
    
- Variations are **human-driven**
    
- You want **maximum ROI early**
    

Example:

```bash
hashcat -a 0 -r best64.rule hashes.txt wordlist.txt
```

---

#### Use **[[Masks]]** When:

- Password policy is known
    
- Structure is consistent
    
- Length and charset are predictable
    
- You want **bounded keyspace**
    

Example:

```bash
hashcat -a 3 hashes.txt ?u?l?l?l?l?d?d?d?d
```

---

#### Use **Hybrid** When:

- You know **part** of the password
    
- Users append predictable data
    
- Rules alone plateau
    
- Masks alone are too large
    

Example:

```bash
hashcat -a 6 wordlist.txt ?d?d?d?d hashes.txt
```

---
### Recommended Escalation Order

1. **Rules (best64 / nsa64)**
    
2. **Hybrid (Dict + digits / years)**
    
3. **Targeted Masks**
    
4. **Large Rulesets (dive / generated)**
    
5. **Random Rules**



#sudad #rules #howto 
[[Rules]]
[[Home]]