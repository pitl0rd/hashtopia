# Chapter X

## Structural and Text-Analytic Analysis of Password Composition

### Abstract

Passwords encode far more than authentication secrets; they embed behavioral, cultural, and contextual signals derived from their creators. By decomposing passwords into observable structural components, defenders can better understand failure modes, population-level risk, and the predictable patterns exploited by attackers. This chapter formalizes a **text-analytic framework for password analysis**, categorizing patterns into **Basic**, **Macro**, and **Micro** levels, and correlates these findings with regional trends and password manager behaviors.

---

## 1. Introduction

Passwords are not random artifacts. They are **human-generated strings**, shaped by memory constraints, policy requirements, language, culture, and habit. As such, password analysis can be treated as a specialized subdomain of **text analytics**, where structure, frequency, and context reveal meaningful information.

This chapter presents a structured methodology for password analysis based on three pattern categories:

- **Basic Patterns** – visually obvious components
    
- **Macro-Patterns** – statistical and structural properties
    
- **Micro-Patterns** – subtle, contextual, and behavioral signals
    

Together, these layers enable systematic interpretation of password datasets and explain why compromise scales rapidly once guessing begins.

---

## 2. Text Analytics Framework for Password Analysis

### 2.1 Overview of Pattern Categories

Password structure can be analyzed across three increasingly granular layers:

**Figure 1. Pattern Analysis Layers**

```
+---------------------+
|   Micro-Patterns    |  Context, habits, themes
+---------------------+
|   Macro-Patterns    |  Length, charset, masks
+---------------------+
|   Basic Patterns    |  Base words, digits, symbols
+---------------------+
```

Each layer provides distinct analytical value and informs different attack and defense strategies.

---

## 3. Basic Pattern Analysis

### 3.1 Definition

Basic patterns are **visually obvious components** identifiable through direct inspection and simple grouping. These typically include:

- Language and base words
    
- Common substitutions (L33T)
    
- Numeric suffixes or prefixes
    
- Predictable special characters
    

### 3.2 Examples

```
R0b3rt2017!
Jennifer1981!
```

**Observed characteristics:**

- Use of personal names (`Robert`, `Jennifer`)
    
- L33T speak substitution (`o → 0`, `e → 3`)
    
- Four-digit numeric suffix resembling a year
    
- Trailing exclamation mark
    

### 3.3 Analytical Implications

These patterns lend themselves to:

- Dictionary-based attacks
    
- L33T substitution rules
    
- Hybrid attacks combining dictionaries with date suffixes
    

**Table 1. Basic Pattern Indicators**

|Indicator|Observation|Analytical Use|
|---|---|---|
|Base word|Personal name|Dictionary seed|
|Digits|4-digit year|Hybrid suffix|
|Symbol|Trailing `!`|Rule-based append|
|Substitution|Common L33T|Rule expansion|

---

## 4. Macro-Pattern Analysis

### 4.1 Definition

Macro-patterns describe **statistical properties** of passwords independent of specific content, including:

- Total length
    
- Character classes used
    
- Mask structure
    
- Repeated constants
    

### 4.2 Examples

```
7482Sacrifice
Solitaire7482
```

**Structural observations:**

- Fixed numeric constant (`7482`)
    
- Word–number inversion
    
- Consistent capitalization
    
- Length ≈ 12 characters
    

### 4.3 Analytical Implications

While the passwords appear long, the **effective search space** is reduced:

- A fixed 4-digit constant lowers uncertainty
    
- Only the dictionary portion varies
    
- Special characters may be absent entirely
    

**Figure 2. Effective Search Space Reduction**

```
Total length: 12
Fixed digits: 4
Variable characters: 8
```

**Table 2. Macro-Pattern Characteristics**

|Property|Value|
|---|---|
|Length|12 ± 1|
|Charset|Lowercase + digits|
|Fixed component|4-digit constant|
|Likely attack|Hybrid (Dict + Digits)|

---

## 5. Micro-Pattern Analysis

### 5.1 Definition

Micro-patterns capture **subtle consistency and contextual meaning**, including:

- Themes (colors, animals, hobbies)
    
- Sequential numbering
    
- Consistent capitalization rules
    
- Personal or cultural associations
    

### 5.2 Examples

```
BlueParrot345
RedFerret789
```

**Observed micro-patterns:**

- Color + animal structure
    
- Title-case capitalization
    
- Sequential three-digit suffixes
    

### 5.3 Analytical Implications

These patterns reveal:

- Likely reuse across accounts
    
- Predictable variant generation
    
- The presence of themed password “families”
    

**Table 3. Micro-Pattern Signals**

|Signal|Example|Inference|
|---|---|---|
|Theme|Color + animal|Personal preference|
|Digits|Sequential|Low entropy|
|Capitalization|Consistent|Habitual rule|
|Structure reuse|Yes|Variant family|

---

## 6. Regional Password Trend Analysis

### 6.1 US / EU / CA Trends

#### Length Distribution

**Table 4. Western Password Length Distribution**

|Length|Percentage|
|---|---|
|7|15%|
|8|27%|
|9|15%|
|10|12%|
|11|4.8%|
|12|4.9%|
|13+|<1%|

#### Character Frequency

- **English text:** `etaoinshrdlcumwfgypbvkjxqz`
    
- **Passwords:** `aeionrlstmcdyhubkgpjvfwzxq`
    

#### Common Masks

**Table 5. Common Western Masks**

|Mask|Description|
|---|---|
|`?l?l?l?l?l?l`|6 lowercase|
|`?l?l?l?l?l?l?d?d`|6 lowercase + 2 digits|
|`?d?d?d?d?d?d`|6 digits|
|`?l x12`|12 lowercase|

---

### 6.2 CN (Chinese) Trends

#### Length Distribution

**Table 6. Eastern Password Length Distribution**

|Length|Percentage|
|---|---|
|7|21%|
|8|22%|
|9|12%|
|10|12%|
|11|4.2%|
|12+|<1%|

#### Observed Differences

- Strong preference for **numeric-only passwords**
    
- Higher digit density
    
- Lower reliance on symbols
    

**Table 7. Common Eastern Masks**

|Mask|Description|
|---|---|
|`?d x8`|8 digits|
|`?d x6`|6 digits|
|`?l x6`|6 lowercase|
|`?l?l?l?d?d?d?d?d?d`|Mixed|

---

## 7. Password Manager Generation Trends

Password managers introduce **non-human structures** that are statistically distinct.

**Table 8. Password Manager Defaults**

|Manager|Length|Charset|Distinguishing Traits|
|---|---|---|---|
|Safari|15|u/l/d + `-`|Grouped segments|
|Dashlane|12|u/l/d|No symbols|
|KeePass|20|u/l/d/s|High entropy|
|LastPass|12|u/l/d|Minimal complexity|
|1Password|24|u/l/d/s|Long, dense|

These passwords resist traditional pattern-based guessing but remain vulnerable if exposed through reuse or endpoint compromise.

---

## 8. Implications for Security Analysis

### 8.1 Why This Matters

- Passwords cluster around **behavioral norms**
    
- Structure dominates guessability more than entropy
    
- Length alone does not imply strength
    
- Cultural and organizational patterns amplify risk at scale
    

### 8.2 Defensive Value

Understanding these structures enables:

- Better password policy design
    
- More accurate breach impact assessment
    
- Improved detection of reuse and variant families
    
- Realistic modeling of attacker success rates
    

---

## 9. Conclusion

Password security failures are rarely cryptographic. They are **behavioral and structural**. By applying text-analytic techniques across Basic, Macro, and Micro layers, defenders gain a predictive understanding of how and why passwords fail. This framework transforms password cracking from a purely technical exercise into a **measured analysis of human behavior at scale**.


[[Password Pattern Analysis|Analysis]]
[[Home]]
#research 