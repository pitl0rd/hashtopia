

# Rule-Based Password Mutation

_(Hashtopia Reference Chapter)_

## Overview

Rule-based attacks transform base password candidates into realistic variants by applying deterministic mutations. Rather than expanding keyspace blindly, rules model human behavior capitalization habits, leetspeak substitutions, suffixing digits, and symbolic decoration.

Rules are most effective when paired with:

- High-quality base wordlists
    
- Observed password patterns
    
- Iterative analysis of cracked results
    

This section documents rule mechanics, generation techniques, validation workflows, and commonly used rule sets across Hashcat and John the Ripper.

---

## Rule Fundamentals

Rules operate as instruction sequences applied to each word in a dictionary. Each rule modifies the input word in a predictable way.

### Basic Rule Examples

|Input Word|Rule|Output|Explanation|
|---|---|---|---|
|password|`$1`|password1|Append digit|
|password|`,...,,...1`|l!password|Prepend symbols|
|password|`so0 sa@`|p@ssw0rd|Leetspeak substitutions|
|password|`c so0 sa@ $1`|P@ssw0rd1|Capitalize + leet + append|
|password|`u r`|DROWSSAP|Uppercase + reverse|

**Key Insight:**  
Rules are evaluated **left to right**, and order matters.

---

## Rule Validation & Debugging

Before launching large-scale attacks, rule behavior should be verified.

### Send Rule Output to STDOUT

```bash
hashcat dict.txt -r rule.txt --stdout
john --wordlist=dict.txt --rules=example --stdout
```

This allows:

- Visual confirmation of transformations
    
- Early detection of logic errors
    
- Faster iteration on custom rules
    

---

## Rule Generation at Scale

### Maskprocessor (hashcat-utils)

Maskprocessor is commonly used to **mass-generate rules** efficiently.

#### Example: Prepend Digit + Symbol

```bash
mp64.bin '"?d "?s' -o rule.txt
```

Generates combinations like:

- `1!password`
    
- `2@password`
    
- `3#password`
    

#### Example: Custom Charset Appending

```bash
mp64.bin -1 aeiou -2 QAZWSX '$?1 $?2 $?d'
```

Produces structured append rules such as:

- `$a $Q $1`
    
- `$e $A $2`
    

**Use Case:**  
High-coverage mutation without hand-writing thousands of rules.

---

## Random Rule Attacks (“Raking”)

### On-the-Fly Random Rules

```bash
hashcat -a 0 -m #type -g <num_rules> hash.txt dict.txt
```

Random rules:

- Explore untested mutation paths
    
- Surface unexpected password structures
    
- Are especially effective after rule exhaustion
    

### Generate Random Rule Files

```bash
generate-rules.bin 1000 42 I ./cleanup-rules.bin 2 > out.txt
```

---

## Capturing Successful Rules

Successful rules provide **actionable intelligence**.

```bash
hashcat -a 0 -m #type \
  --debug-mode=1 \
  --debug-file=debug.txt \
  hash.txt -r rule.txt
```

Collected data enables:

- Rule effectiveness ranking
    
- Future rule pruning
    
- Dataset-specific optimization
    

---

## Included Rule Sets (Hashcat)

|Rule File|Approx. Rules|
|---|---|
|best64.rule|77|
|leetspeak.rule|29|
|rockyou-30000.rule|30,000|
|d3ad0ne.rule|34,101|
|dive.rule|99,092|
|generated.rule|14,733|
|generated2.rule|65,117|
|toggles5.rule|4,943|
|Incisive-leetspeak.rule|15,487|
|unix-ninja-leetspeak.rule|3,073|

**Operational Guidance:**  
Start small → escalate selectively.  
Large rule sets are powerful but expensive.

---

## Included Rule Sets (John the Ripper)

|Rule Group|Approx. Rules|
|---|---|
|KoreLogic|~7,074,000|
|Jumbo|226|
|All (Jumbo + KoreLogic)|~7.07M|
|Wordlist|25|
|Single|169|
|NT|14|

**Note:**  
John’s strength lies in **deep combinatorial rule depth**, while Hashcat emphasizes **GPU efficiency**.

---

## Rule Plan Analysis

Rule effectiveness varies per dataset. Below are **top-performing rules** identified during analysis.

### Top 10 – dive.rule

```
C
1
u
T0
$1
}}}}
p3
[
$.
l
```

### Top 10 – best64.rule

```
r
u
T0
$0
$1
$2
$3
$4
$5
77
```

### Top 10 – rockyou.rule

```
$1
r
$2
$1 $2 $3
$1 $2
$3
$7
Al
$1 $3
```

**Insight:**  
A small subset of rules often accounts for the majority of successful cracks.

---

## Notable Community Rule Sets

### D3adH0b0 / Hob0 Rules

High-coverage, time-intensive rule sets.

```bash
hashcat -a 0 -m 1000 hashes.txt \
  wordlists/english.txt -r d3adhob0.rule
```

Smaller, fast subset:

```bash
hashcat -a 0 -m 1000 hashes.txt \
  wordlists/rockyou.txt -r hob064.rule
```

---

### KoreLogic Rules

Originally developed for John the Ripper and later translated for Hashcat.

- Contest-proven
    
- Highly aggressive
    
- Best suited for long-running jobs
    

---

### OneRuleToRuleThemAll (NotSoSecure)

Composite rule set derived from:

- Hob0Rules
    
- KoreLogic
    
- NSAKEY
    
- Hashcat generated rules
    

**Use Case:**  
Maximum coverage when time and compute allow.

---

## NSAKEY Rules

|Rule Set|Description|
|---|---|
|NSAKEYv1diverule|Initial dive.rule competitor|
|NSAKEYv2diverule|Improved successor|
|nsa64rule|First 64 rules, ~40% coverage on weak datasets|

```bash
head -n 64 NSAKEYv2diverule.rule > nsa64.rule
```

---

## Operational Takeaways

- Rules **amplify intelligence**, not brute force
    
- Capture and reuse **successful mutations**
    
- Small, curated rule sets outperform brute megasets early
    
- Rule analysis feeds back into **password policy, detection, and remediation**
    
[[Rules]]
[[Home]]
