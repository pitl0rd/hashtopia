
## 1) [[PACK - Password Analysis and Cracking Kit]]

**Tool:** PACK (Password Analysis and Cracking Kit)  
**Repo:** iphelix/pack ([GitHub](https://github.com/iphelix/pack "GitHub - iphelix/pack: PACK (Password Analysis and Cracking Kit)"))

### What it is

PACK is a small toolkit for **password research + candidate-space engineering**: you feed it password data and it helps you derive **masks, statistics, and wordlist transforms** that can be consumed by cracking/analysis workflows. ([GitHub](https://github.com/iphelix/pack "GitHub - iphelix/pack: PACK (Password Analysis and Cracking Kit)"))

### Where it fits in a Hashtopia workflow

**Collection → Normalize → Analyze → Generate Candidates → Run Jobs → Measure Coverage**

PACK lives in the **Analyze → Generate Candidates** segment: it’s how you turn “a pile of known passwords” into **repeatable, structured candidate strategies**.

### Core workflow (conceptual)

1. **Input:** a plaintext password corpus (already authorized/approved for research use).
    
2. **Analysis:** compute length/charset/digit placement patterns, etc.
    
3. **Output:** artifacts like:
    
    - **masks** (structure-first guessing)
        
    - **stats models** (frequency-driven guessing)
        
    - **cleaned dictionaries** (better inputs for combinator/PCFG/PRINCE)
        

### Practical outputs you’ll typically track

- **Top masks** (e.g., common shapes like `?u?l?l?l?l?l?d?d`)
    
- **Position bias** (digits at end, cap-first, symbol-last patterns)
    
- **Token families** (names, sports, seasons, local org themes)
    

### Operational tips

- **Always version** the derived artifacts (mask sets, cleaned lists) with:
    
    - source name (lab dataset / engagement ID)
        
    - date
        
    - preprocessing notes
        
- Treat generated artifacts as **sensitive**: even “just masks” can encode behavioral/PII-adjacent tendencies.

---

## 2) [[PIPAL - Password Intelligence and Pattern Analysis]]

**Tool:** Pipal  
**Repo:** digininja/pipal ([GitHub](https://github.com/digininja/pipal "GitHub - digininja/pipal: Pipal, THE password analyser"))

### What it is

Pipal is a **password analysis/reporting** tool: it summarizes a password set and produces stats like **length distribution, charset usage, top base words, suffix patterns**, etc. ([GitHub](https://github.com/digininja/pipal "GitHub - digininja/pipal: Pipal, THE password analyser"))

### Why you use it

Use Pipal when you need to answer questions like:

- “What does this corpus _look like_ structurally?”
    
- “What are the top 20 base words / years / suffixes?”
    
- “Do we have strong evidence of policy-driven patterns (caps-first, digit-last)?”
    
- “What should we prioritize in candidate generation?”

### Typical Hashtopia workflow placement

**Ingest → Sanitize → Analyze (Pipal) → Strategy Draft → Execute Jobs → Iterate**

### Outputs you should capture in your research notes

- **Length histogram**
    
- **Top N base words** (and whether they’re names/brands/org terms)
    
- **Common suffixes** (years, `!`, seasonal patterns)
    
- **Charset mix** (lower-only vs mixed-case vs digits-heavy)
    
- **Repeat behavior signals** (small number of structures dominating)

### Recommended practice

- Run Pipal on:
    
    - **full corpus**
        
    - plus **segmented corpora** (by org unit, region, language, system)
        
- Store results as artifacts alongside:
    
    - scope notes
        
    - preprocessing method
        


---
[[Resources]]
[[Home]]


