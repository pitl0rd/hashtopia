
This section catalogs commonly used **password dictionaries and wordlists** that are frequently referenced in password research, analysis, and defensive studies. Each entry includes context on how and why the list is typically used in research workflows (pattern analysis, entropy modeling, user behavior studies, and tooling validation).

---
## CAPSOP

**URL:** [https://wordlists.capsop.com/](https://wordlists.capsop.com/)

**Description:**  
CAPSOP provides curated wordlists derived from real-world password leaks, optimized for statistical coverage rather than raw size. Lists are often segmented by likelihood and structure, making them useful for **probability-based analysis**, benchmarking cracking strategies, and studying human password selection bias.

**Research Use:**

- Baseline password likelihood modeling
    
- Comparative efficiency testing of attack strategies
    
- Evaluating password policy effectiveness

---

## EFF Wordlists

**URLs:**

- Long list (7,776 words): [https://www.eff.org/files/2016/07/18/eff_large_wordlist.txt](https://www.eff.org/files/2016/07/18/eff_large_wordlist.txt)
    
- Short list (1,296 words): [https://www.eff.org/files/2016/09/08/eff_short_wordlist_l.txt](https://www.eff.org/files/2016/09/08/eff_short_wordlist_l.txt)

**Description:**  
Created by the Electronic Frontier Foundation, these lists were designed for **passphrase generation**, not cracking. Words are intentionally selected to maximize memorability while preserving entropy.

**Research Use:**

- Passphrase entropy modeling
    
- Comparison between passphrases vs traditional passwords
    
- Defensive education and usability studies

---

## HASHES.ORG – Found Password Lists

**URL:** [https://hashes.org/left.php](https://hashes.org/left.php)

**Description:**  
Hashes.org publishes recovered plaintext passwords derived from public hash submissions. These lists represent **validated, real-world password choices**, often clustered by breach source.

**Research Use:**

- Ground-truth password behavior analysis
    
- Baseword clustering and reuse studies
    
- Training datasets for pattern analysis tools (e.g., Pipal, PACK)

---

## Have I Been Pwned (HIBP) Passwords

**URL:** [https://haveibeenpwned.com/passwords](https://haveibeenpwned.com/passwords)

**Description:**  
HIBP provides large-scale datasets of breached passwords in hashed form (MD5, SHA-1). While plaintext is not provided, the corpus represents one of the most comprehensive views of **password reuse at internet scale**.

**Research Use:**

- Password reuse and prevalence studies
    
- Validation of password screening controls
    
- Defensive monitoring and deny-list research

---

## Probable Wordlists

**URL:** [https://github.com/berzerk0/Probable-Wordlists](https://github.com/berzerk0/Probable-Wordlists)

**Description:**  
Probable Wordlists are statistically generated from breached password datasets, ordered by likelihood. They prioritize **coverage efficiency**, not completeness.

**Research Use:**

- Studying diminishing returns in dictionary size
    
- Optimizing wordlist ordering
    
- Measuring marginal gains in password recovery rates

---

## Skull Security Wordlists

**URL:** [https://wiki.skullsecurity.org/index.php?title=Passwords](https://wiki.skullsecurity.org/index.php?title=Passwords)

**Description:**  
A long-standing archive of password lists, including legacy datasets, thematic wordlists, and historical breach artifacts.

**Research Use:**

- Longitudinal password trend analysis
    
- Studying legacy password construction habits
    
- Comparative analysis across time periods

---

## Unix-Ninja Password DNA Dictionary

**URL:** [https://www.unix-ninja.com/p/Password_DNA](https://www.unix-ninja.com/p/Password_DNA)

**Description:**  
This dictionary is derived from **Password DNA analysis**, focusing on basewords, mutations, and structural reuse rather than raw password strings.

**Research Use:**

- Structural decomposition of passwords
    
- Baseword mutation modeling
    
- Rule and mask optimization research

---

## WEAKPASS

**URL:** [http://weakpass.com/](http://weakpass.com/)

**Description:**  
WEAKPASS aggregates and analyzes public password dumps, producing refined dictionaries optimized for coverage and efficiency.

**Research Use:**

- Comparative dictionary benchmarking
    
- Pattern density analysis
    
- Evaluating real-world password predictability

---

## rockyou.txt

**Description:**  
One of the most widely referenced password datasets, originating from the RockYou breach. It represents a **canonical snapshot of consumer password behavior**.

**Research Use:**

- Baseline password studies
    
- Tool validation and reproducibility
    
- Educational demonstrations of weak password habits

---

## rocktastic.txt

**Description:**  
A curated enhancement of rockyou-derived data, often reordered or filtered to improve likelihood-based coverage.

**Research Use:**

- Incremental improvement testing over baseline datasets
    
- Evaluating ranking and pruning strategies

---

## Crackstation Wordlist

**URL:** [https://crackstation.net/](https://crackstation.net/)

**Description:**  
A large composite wordlist built from multiple sources, intended to maximize coverage of common passwords, phrases, and mutations.

**Research Use:**

- Upper-bound testing of dictionary-based approaches
    
- Studying tradeoffs between list size and effectiveness
    
- Benchmarking against smaller, probability-optimized lists

---

## Summary Perspective

From a Hashtopia-style research lens:

- **Raw size does not equal effectiveness**
    
- High-value wordlists reflect **human behavior**, not randomness
    
- The most useful datasets enable:
    
    - Pattern discovery
        
    - Structural analysis
        
    - Comparative methodology validation

These wordlists should be treated as **observational datasets**, not weapons, but inputs for understanding how humans choose secrets, and how systems can be designed to defend against predictable behavior.

[[Home]]

#wordlists #dictionary 
