
**Category:** Password Analysis & Pre-Cracking Preparation  
**Purpose:** PACK helps you analyze large password datasets and derive **statistics, masks, rules, and patterns** that can improve targeted cracking strategies. It does **not crack passwords itself** but generates highly useful inputs for cracking tools like **Hashcat**. ([GitHub](https://github.com/iphelix/pack "iphelix/pack: PACK (Password Analysis and Cracking Kit)"))

---

## What PACK Is

PACK (Password Analysis and Cracking Kit) is a collection of command-line utilities designed to analyze password lists and produce structured metadata that informs cryptanalysis strategies. It generates:

- **Statistical summaries** of password characteristics
    
- **Pattern-based masks** reflecting common structures
    
- **Rule sets** derived from real password similarity
    
- **Policy-based mask templates** ([GitHub](https://github.com/iphelix/pack "iphelix/pack: PACK (Password Analysis and Cracking Kit)"))

---

## Why Use PACK

Rather than sending every candidate blindly into a cracking engine, PACK helps you answer:

- What **lengths** are most common in this user population?
    
- Which **character-set patterns** dominate real passwords?
    
- Which **mask patterns** match the target dataset best?
    
- What **rules** can be derived from observed similarities?
    

---

## Installation

PACK is a Python utility suite that works with Python 3. Before installing, ensure you have Python 3 and the **pyenchant** dictionary library.

1. Clone the official repository:
    
    ```bash
    git clone https://github.com/iphelix/pack.git
    cd pack
    ```
    
2. Install dependencies (example for Python 3):
    
    ```bash
    python3 -m pip install pyenchant
    ```
    

> If pyenchant installation fails due to missing dictionaries, you may need to install or configure language providers (e.g., `aspell` dictionaries). ([Knowledge Base (KB)](https://kb.offsec.nl/tools/hash-cracking/pack/ "PACK - Knowledge Base (KB)"))

---

## Core PACK Commands

PACK includes several utilities. Here’s how they fit together:

---

### **1. StatsGen - Statistical Summary**

Use **statsgen.py** to generate high-level statistics from a password list.

**Purpose:**  
Shows distributions for **lengths, character-sets, masks**, and other key characteristics of the dataset.

**Example:**

```bash
python statsgen.py rockyou.txt
```

Outputs include:

- Most common password **lengths**
    
- Character-set statistics (e.g., loweralphanum, numeric)
    
- **Simple masks** (e.g., stringdigit)
    
- **Advanced masks** (Hashcat mask format) ([GitHub](https://github.com/iphelix/pack? "iphelix/pack: PACK (Password Analysis and Cracking Kit)"))
    

This output helps you understand **which patterns occur most**, and thus where cracking should concentrate.

---

### **2. MaskGen - Pattern Mask Generation**

Use **maskgen.py** to convert statistics into prioritized **Hashcat mask templates**.

**Purpose:**  
From statsgen output, generate the most _efficient_ masks given time or resource constraints.

**Example:**

```bash
python maskgen.py rockyou.masks --targettime 600 --optindex
```

Key options:

- `--targettime <seconds>` - desired cracking target duration
    
- `--optindex` - sort by optimal index (best balance of coverage vs time) ([GitHub](https://github.com/iphelix/pack?utm_source=chatgpt.com "iphelix/pack: PACK (Password Analysis and Cracking Kit)"))
    

Resulting masks are saved and can be used directly in cracking workflows.

---

### **3. RuleGen - Rule Extraction**

Use **rulegen.py** to derive **probabilistic modification rules** from a password list.

**Purpose:**  
Automatically create a ruleset capturing common transformation behaviors (e.g., substitution, inserts), which you can feed to Hashcat.

**Example:**

```bash
python rulegen.py rockyou.txt
```

Rules generated help crack passwords with real-world human mangling, not just basic dictionary matches. ([Knowledge Base (KB)](https://kb.offsec.nl/tools/hash-cracking/pack/ "PACK - Knowledge Base (KB)"))

---

### **4. PolicyGen - Policy-Driven Masks**

Use **policygen.py** to generate masks that align with specific **password policies** (e.g., minimum uppercase, digits, symbols).

**Purpose:**  
When targeting a population with known password rules, PolicyGen creates masks that obey those constraints while still being efficient.

**Example:**

```bash
python policygen.py --policy "<policy_spec>" input.txt
```

This ensures cracking avoids patterns users couldn’t have used due to enforced requirements.

---

## Typical Workflow

### **Step 1 - Collect Representative Password Data**

Gather a large sample of relevant passwords (e.g., from a same-industry breach or historical data).

### **Step 2 - Analyze with StatsGen**

```bash
python statsgen.py sample.txt -o sample.masks
```

This produces `sample.masks` with mask frequencies and other metadata.

### **Step 3 - Generate Optimized Masks**

```bash
python maskgen.py sample.masks --targettime 3600 --optindex -o generated.hcmask
```

This yields `generated.hcmask`, a prioritized mask list tuned for a 1-hour cracking window.

### **Step 4 - Generate Rules**

```bash
python rulegen.py sample.txt
```

This outputs rule files usable with Hashcat.

### **Step 5 - Use with Cracking Engines**

Combine generated masks and rules with Hashcat:

```bash
hashcat -m <mode> -a 3 hashfile.txt generated.hcmask
hashcat -m <mode> -a 0 hashfile.txt wordlist.txt -r analysis.rule
```

Results are far more targeted than blind dictionary attacks.

---

## Best Practices

**Choose relevant sample data:** PACK’s insights are only as good as the input. Using unrelated breaches may bias patterns. ([GitHub](https://github.com/iphelix/pack "iphelix/pack: PACK (Password Analysis and Cracking Kit)"))  
**Target time constraints:** Always specify reasonable `--targettime` when creating masks to balance coverage vs cracking effort.  
**Combine with intelligence:** Use masks + rules together for best practical coverage.

---

## How PACK Fits Into the Cracking Ecosystem

PACK is part of an **analysis pipeline** that precedes actual cracking. Its value lies in turning real password behavior into structured cracking inputs, not in performing the cracking itself.

By measuring distributions and extracting patterns, you reduce guessable space and improve probability mass alignment with real users,  a principle foundational to effective password analysis. ([GitHub](https://github.com/iphelix/pack "iphelix/pack: PACK (Password Analysis and Cracking Kit)"))


[[Password Pattern Analysis|Analysis]]
[[Home]]


#sudad #tools #fundamentals 