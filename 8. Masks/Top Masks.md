

Masks are a way to **encode human behavior into a constrained search space**.

They are not guesses at individual passwords, they are hypotheses about **how people tend to structure passwords** under real-world conditions.

  

This page documents a set of empirically derived masks based on analysis of a **2.7 billion–record password corpus**, along with guidance on how and why these masks matter.

---

## **Why These Masks Exist**

  

When large password datasets are analyzed at scale, certain structures repeat with extreme regularity.

These repetitions are not accidental, they reflect:

- Minimal-effort compliance with password policies
    
- Cultural and regional numeric preferences
    
- Language-specific defaults
    
- Predictable placement of digits and characters
    

  

The masks below represent **high-density regions of the human password space**.

  

> **Key Result:**

> These masks alone recovered **~6% (≈162 million)** plaintext passwords from the dataset.

  

That recovery rate was achieved **without rules, dictionaries, or behavioral labeling** only structural constraints.

---

## **US / Western-Centric Mask Plans**

  

These masks reflect trends commonly observed in US, EU, and similar regions where:

- Alphabetic characters dominate
    
- Digits are appended rather than interleaved
    
- Lowercase is preferred unless policy forces otherwise
    

|**Mask**|**Description**|
|---|---|
|?l?l?l?l?l?l|6 lowercase letters|
|?l?l?l?l?l?l?l|7 lowercase letters|
|?l?l?l?l?l?l?l?l|8 lowercase letters|
|?l?l?l?l?l|5 lowercase letters|
|?l?l?l?l?l?l?l?l?l|9 lowercase letters|
|?l?l?l?l?l?l?l?l?l?l|10 lowercase letters|
|?l?l?l?l?l?l?l?l?l?l?l?l|12 lowercase letters|
|?d?d?d?d?d?d|6 digits|
|?d?d?d?d?d?d?d?d|8 digits|
|?l?l?l?l?l?d?d|5 letters + 2 digits|
|?l?l?l?l?l?l?d?d|6 letters + 2 digits|
|?l?l?l?l?l?l?l?l?d?d|8 letters + 2 digits|
|?l?l?l?l?l?l?d?d?l?l?l?l|Letters → digits → letters|
|?d?d?d?d?d?d?d?d?l?l?l?l|Digits followed by letters|

### **Observations**

- Digits overwhelmingly appear at **edges**, not in the middle
    
- Lowercase dominates unless forced otherwise
    
- Longer passwords often remain **structurally simple**, not complex
    

---

## **Asia / Digit-Heavy Mask Plans**

  

These masks reflect datasets where numeric passwords are significantly more common, often due to:

- Mobile-first authentication habits
    
- Numeric keypad familiarity
    
- Different cultural memorability patterns

|**Mask**|**Info**|
|---|---|
|?d?d?d?d?d?d|6-Digits|
|?d?d?d?d?d?d?d|7-Digits|
|?d?d?d?d?d?d?d?d|8-Digits|
|?d?d?d?d?d?d?d?d?d|9-Digits|
|?d?d?d?d?d?d?d?d?d?d|10-Digits|
|?d?d?d?d?d?d?d?d?d?d?d|11-Digits|
|?d?d?d?d?d?d?d?d?d?d?d?d|12-Digits|
|?l?l?l?l?l?l|6-Lowercase|
|?l?l?l?l?l?l?l|7-Lowercase|
|?l?l?l?l?l?l?l?l|8-Lowercase|
|?l?l?l?l?l?l?l?l?l|9-Lowercase|
|?l?l?l?l?l?l?l?l?l?l|10-Lowercase|
|?l?l?d?d?d?d?d?d|2-Lowercase + 6-Digits|
|?l?l?l?d?d?d?d?d?d|3-Lowercase + 6-Digits|

---

### **Context & Takeaways**

- These masks heavily favor **numeric-only passwords**, reflecting:
    
    - Mobile-first input patterns
        
    - PIN reuse
        
    - Numeric identifiers doubling as passwords
        
    
- Mixed alpha–numeric masks are present but far less dominant than in Western datasets.
    
- Lowercase-only passwords remain common where Latin input is used, but digits still dominate.
    

  

These masks are **statistically derived**, not speculative.

They consistently recover large volumes of weak credentials when applied early in analysis pipelines.

  

If you want, next we can:

- Add **policy-aligned variants** (min/max length, required digits)
    
- Layer **increment strategies** (--increment)
    
- Convert these into a **.hcmask plan** ordered by expected yield
### **Observations**

- Pure numeric passwords remain extremely common
    
- Mixed masks often place letters **before** digits
    
- Length increases do not imply entropy increases
    

---

## **Why Simple Masks Still Matter**

  

Many of these masks look trivial.

That is precisely why they work.

- Users optimize for **minimum effort**
    
- Policy compliance does not equal randomness
    
- Length ≠ strength when structure is predictable

---

## **When to Use These Masks**

  

These masks are most effective when:

- Targeting **weak or legacy password policies**
    
- Performing **baseline password hygiene analysis**
    
- Establishing a **floor of expected compromise**
    
- Studying **human password construction behavior**
    
- Building **policy-aware variants** (e.g., forcing uppercase or symbols)
    

  

They also serve as an excellent foundation for:

- Hybrid mask + dictionary attacks
    
- Rule amplification
    
- Comparative analysis across regions or datasets
    

---

## **Key Takeaways**

- Masks represent **structure**, not guesses
    
- Large-scale data confirms that **simple structures dominate**
    
- Low-hanging fruit is always present, even at massive scale
    
- Mask attacks are one of the **purest measurements** of human password behavior

---
#sudad

[[Masks]]
[[Home]]