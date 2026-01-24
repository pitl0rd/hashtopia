
This section supplements the PACK user guide with:

1. A **command-line cheat sheet**
    
2. A **workflow diagram (analysis → cracking)**
    
3. A **concrete case study** showing how PACK informs real cracking decisions
    

---

## 1. PACK Command-Line Cheat Sheet

This cheat sheet focuses on the **most commonly used flags and workflows** rather than exhaustively listing every option.

---

### **statsgen.py - Dataset Analysis**

**Purpose:**  
Generate statistical summaries from a password list.

```bash
python statsgen.py <password_list>
```

Common options:

- `-o <file>` - Write output to a file
    
- `--encoding <enc>` - Specify encoding (e.g., latin-1)
    
- `--minlen <n>` - Ignore passwords shorter than length `n`
    
- `--maxlen <n>` - Ignore passwords longer than length `n`
    

**Output Includes:**

- Length distribution
    
- Character-set distribution
    
- Simple masks (e.g., `stringdigit`)
    
- Hashcat-style advanced masks

---

### **maskgen.py - Mask Optimization**

**Purpose:**  
Convert statistical mask data into **prioritized Hashcat masks**.

```bash
python maskgen.py <stats_file>
```

Common options:

- `--targettime <seconds>` - Target cracking duration
    
- `--optindex` - Optimize for probability/time balance
    
- `-o <file>` - Output mask file
    

Example:

```bash
python maskgen.py sample.masks --targettime 1800 --optindex -o optimized.hcmask
```

---

### **rulegen.py - Rule Extraction**

**Purpose:**  
Generate password-mangling rules based on observed transformations.

```bash
python rulegen.py <password_list>
```

Common options:

- `--minrules <n>` - Minimum number of rules to generate
    
- `--maxrules <n>` - Cap rule generation
    

Output:

- One or more `.rule` files usable by Hashcat

---

### **policygen.py - Policy-Driven Masks**

**Purpose:**  
Generate masks that comply with known password policies.

```bash
python policygen.py --policy "<policy>" <password_list>
```

Used when:

- Minimum length is enforced
    
- Character-class requirements are known
    
- Legacy complexity rules are present
    

---

## 2. PACK Workflow Diagram (Analysis → Cracking)

This diagram shows where PACK fits into a **structured password analysis pipeline**. This Pipeline guides the iterative loop that you will often find yourself using as you begin to 'map' the keyspace.

```
          ┌────────────────────┐
          │ Password Dataset   │
          │ (breach / sample)  │
          └─────────┬──────────┘
                    │
                    ▼
          ┌────────────────────┐
          │ statsgen.py        │
          │ - Lengths          │
          │ - Charsets         │
          │ - Masks            │
          └─────────┬──────────┘
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
┌────────────────┐     ┌────────────────┐
│ maskgen.py     │     │ rulegen.py     │
│ Optimized      │     │ Behavioral     │
│ Mask Lists     │     │ Rule Sets      │
└───────┬────────┘     └───────┬────────┘
        │                      │
        └───────────┬──────────┘
                    ▼
          ┌────────────────────┐
          │ Cracking Engine    │
          │ (Hashcat / JtR)    │
          └─────────┬──────────┘
                    │
                    ▼
          ┌────────────────────┐
          │ Measured Outcomes  │
          │ - Time to failure  │
          │ - Coverage         │
          │ - Residual risk    │
          └────────────────────┘
```

**Key Insight:**  
PACK **does not crack passwords**, it **shapes probability** before cracking begins. With an idea of the probable keyspace, and how to cut a path through it, you have substantially increased the statistical likelihood of success.

---

## 3. Case Study - Using PACK to Improve Cracking Efficiency

### **Scenario**

You are performing a controlled assessment and have:

- 50,000 NTLM hashes
    
- No plaintext passwords yet
    
- Access to a large, representative password dataset from a similar environment
    

Your goal is **not maximum recovery**, but to **measure policy effectiveness and failure patterns** that will further inform your experiments and test cases.

---

### **Step 1 - Analyze Password Behavior**

```bash
python statsgen.py reference_passwords.txt -o reference.masks
```

Findings:

- 68% of passwords are **8–10 characters**
    
- Majority follow patterns like:
    
    - `CapitalizedWord + 2–4 digits`
        
    - `Season + Year + !`
        
- Very few mixed-symbol placements
    

---

### **Step 2 - Generate Time-Bound Masks**

You want results within **30 minutes**.

```bash
python maskgen.py reference.masks --targettime 1800 --optindex -o thirty_minute.hcmask
```

Outcome:

- Mask list heavily weighted toward:
    
    - `?u?l?l?l?l?d?d`
        
    - `?u?l?l?l?l?l?d?d?d`
        
- Low-probability masks excluded
    

---

### **Step 3 - Extract Behavioral Rules**

```bash
python rulegen.py reference_passwords.txt
```

Generated rules reflect:

- Appending digits
    
- Capitalizing first letter
    
- Symbol suffixes
    

---

### **Step 4 - Execute Cracking with Intent**

```bash
hashcat -m 1000 -a 3 hashes.txt thirty_minute.hcmask
hashcat -m 1000 -a 0 hashes.txt wordlist.txt -r generated.rule
```

---

### **Measured Results**

- 22% of hashes recovered in under 30 minutes
    
- Nearly all recovered passwords matched **top 5 mask patterns**
    
- Length mattered more than character diversity
    
- Complexity rules did **not** materially slow failure
    

---

### **Security Conclusion**

The value was **not the cracked passwords**, but the insight:

- Password policy created **predictable structure**
    
- Attack cost concentrated early
    
- Marginal gains flattened rapidly
    

This informs:

- Policy redesign
    
- MFA prioritization
    
- Privilege isolation decisions
    
- Risk communication to stakeholders

[[Password Pattern Analysis|Analysis]]
[[Home]]
#sudad #Resources #tools 


