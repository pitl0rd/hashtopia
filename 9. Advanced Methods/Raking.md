Rule Harvesting Methodology

**Raking** is an iterative analysis technique used to **discover which rules actually work** against a given dataset by observing real cracking outcomes instead of guessing in advance.
Rather than treating rules as a static asset, raking treats them as **experimental variables** that can be measured, refined, and re-used.
This methodology is most commonly applied with **Hashcat**, but the conceptual approach is portable to other cracking frameworks.

---

## **What Raking Is (Conceptually)**

  

At its core, raking answers a simple question:

  

> _Which base words + transformations are actually producing results in this dataset?_

  

Raking does this by:

- Running **large numbers of automatically generated rules**
    
- Capturing **successful rule applications**
    
- Extracting:
    
    - Base words
        
    - Rules that worked
        
    - Final transformed passwords
        
    
- Feeding those results back into future attacks
    

  

Over time, this creates **dataset-specific intelligence** instead of relying on generic rule packs.

---

## **Why Raking Works**

  

Traditional rule attacks assume:

- You know which rules matter
    
- You know which base words matter
    
- You know the right order to apply them
    

  

Raking flips this around:

- Let the attack run
    
- Observe what succeeds
    
- Promote _empirically proven_ rules and words
    

  

This is especially effective against:

- Large, noisy datasets
    
- Mixed password policies
    
- Environments with user-specific behavior
    
- Long-running cracking campaigns
    

---

## **High-Level Workflow**

1. **Run a large automated rule attack**
    
2. **Log every successful transformation**
    
3. **Extract components from debug output**
    
4. **Analyze frequency and effectiveness**
    
5. **Re-apply the best performers**
    

  

Each loop improves signal quality and reduces wasted keyspace.

---

## **Step-by-Step Raking Process**

  

### **Step 1: Initial Raking Pass**

  

Run a fast hash type with:

- Automatic rule generation (-g)
    
- Debug logging enabled
    

```
hashcat -a 0 -m #type -w 3 hash.txt wordlists/* \
-g 100000 \
--debug-mode=4 \
--debug-file=nodename.debug
```

**What this does:**

- Loops across multiple wordlists
    
- Generates rules on the fly
    
- Logs every successful guess with full context
    

  

This step is intentionally **broad and noisy**.

---

### **Step 2: Extract Base Words**

```
cut -d: -f1 < nodename.debug > nodename.base
```

**Result:**

- A list of dictionary inputs that actually mattered
    
- Often much smaller than the original wordlists
    
- Strong candidates for:
    
    - Re-use
        
    - Targeted dictionaries
        
    - PRINCE or combinator inputs
        
    

---

### **Step 3: Extract Successful Rules**

```
cut -d: -f2 < nodename.debug > nodename.rule
```

**Result:**

- A dataset-specific rule list
    
- Reflects _real_ user behavior
    
- Can be:
    
    - Deduplicated
        
    - Frequency-ranked
        
    - Trimmed to top-N performers
        
    

  

This is how **generated2.rule** was originally built.

---

### **Step 4: Extract Final Passwords**

```
cut -d: -f3- < nodename.debug > nodename.final
```

**Result:**

- Ground-truth cracked passwords
    
- Useful for:
    
    - Pattern analysis
        
    - Mask derivation
        
    - Markov training
        
    - PCFG or ML research
        
    

---

## **Iterative Refinement Loop**

  

After the first raking pass:

1. Re-run attacks using:
    
    - nodename.base as a new dictionary
        
    - nodename.rule as a curated rule set
        
    
2. Generate a **new debug file**
    
3. Compare effectiveness across iterations
    
4. Retain only high-yield components
    

  

Over time, the attack becomes:

- Smaller
    
- Faster
    
- More precise
    
- More explainable
    

---

## **Measuring Effectiveness**

  

Common metrics to track:

- Rule frequency of success
    
- Base word reuse rate
    
- Cracks per million guesses
    
- Marginal gains per iteration
    

  

This allows you to:

- Drop unproductive rules
    
- Promote dominant transformations
    
- Stop runs earlier with confidence
    

---

## **When to Use Raking**

  

Raking is most valuable when:

- You have **time**, not just speed
    
- You are attacking **large datasets**
    
- You want **long-term improvement**
    
- You are building reusable assets
    

  

It is less useful for:

- One-off quick wins
    
- Very small hash lists
    
- Extremely slow hash types (unless scoped carefully)
    

---

## **Key Takeaways**

- Raking turns cracking into **measurement**, not guesswork
    
- Rules should be _discovered_, not assumed
    
- Debug output is an **intelligence source**
    
- The best rules are **dataset-specific**
    
- Iteration matters more than brute force

---
[[Advanced Compositional Attacks]]
[[Home]]