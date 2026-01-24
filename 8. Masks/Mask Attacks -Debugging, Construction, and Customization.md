
Mask attacks are used when **structure is known or suspected**, but **specific characters are unknown**.

They provide a controlled way to explore password space without the overhead of full brute force or large rule sets.

  

This page covers:

- Verifying mask output
    
- Constructing mask attacks in Hashcat and John
    
- Hybrid mask usage
    
- Custom charset optimization
    

---

## **Debugging and Verifying Mask Output**

  

Before running a cracking session, it is often useful to **validate what a mask actually produces**.

This prevents wasted time exploring unintended keyspace.

  

### **Hashcat**

```
hashcat -a 3 ?a?a?a?a --stdout
```

### **John the Ripper**

```
john --mask=?a?a?a?a --stdout
```

**Why this matters:**

Seeing the generated candidates confirms:

- Character classes are correct
    
- Length is correct
    
- Ordering matches expectations
    

---

## **Hashcat Mask Attack Creation**

  

### **General Syntax**

```
hashcat -a 3 -m <hash-type> hash.txt <mask>
```

Mask attacks (-a 3) enumerate candidates directly from the mask definition.

---

### **Full Brute Force (Fixed Length)**

  

Brute-force all possible combinations exactly **7 characters long**:

```
hashcat -a 3 -m <hash-type> hash.txt ?a?a?a?a?a?a?a
```

---

### **Incremental Mask (1 → N Length)**

  

Brute-force all combinations **from length 1 to 7**:

```
hashcat -a 3 -m <hash-type> hash.txt -i ?a?a?a?a?a?a?a
```

**Use when:**

Password length is unknown, but expected to be short.

---

### **Structured Mask Examples**

  

Uppercase first letter, three unknown characters, ending in two digits:

```
hashcat -a 3 -m <hash-type> hash.txt ?u?a?a?a?d?d
```

Known prefix with unknown suffix:

```
hashcat -a 3 -m <hash-type> hash.txt secret?a?a?a?a
```

---

## **Hybrid Mask Attacks (Hashcat)**

  

Hybrid attacks combine **dictionary intelligence** with **mask-based entropy**.

  

### **Mask + Wordlist (Left Mask, Right Dictionary)**

  

Example: 123!Password

```
hashcat -a 7 -m <hash-type> hash.txt ?a?a?a?a dict.txt
```

### **Wordlist + Mask (Left Dictionary, Right Mask)**

  

Example: Password123!

```
hashcat -a 6 -m <hash-type> hash.txt dict.txt ?a?a?a?a
```

**Why hybrids matter:**

They capture common human behavior:

- Appending digits
    
- Prepending symbols
    
- Mixing memorized words with minimal entropy
    

---

## **Hashcat Custom Character Sets**

  

Hashcat allows up to **four custom charsets**:

```
-1  -2  -3  -4
```

These dramatically reduce keyspace when patterns are known.

---

### **Example: Restricted Starting Characters**

  

Passwords starting with aA bB cC, four unknown characters, ending in a digit:

```
hashcat -a 3 -m <hash-type> hash.txt -1 abcABC ?1?a?a?a?a?d
```

---

### **Example: Letter + Digits + Symbol**

  

Upper/lowercase start, four digits, ending with ! @ $:

```
hashcat -a 3 -m <hash-type> hash.txt -1 ?u?l -2 !@$ ?1?d?d?d?d?2
```

---

### **Using All Four Custom Charsets**

  

Example candidate: pow!12er

```
hashcat -a 3 -m <hash-type> hash.txt \
-1 qwer -2 poiu -3 123456 -4 !@#$% \
?2?2?1?4?3?3?1?1
```

**Takeaway:**

Custom charsets turn masks from **brute force** into **pattern exploration**.

---

## **John the Ripper Mask Attacks**

  

### **General Syntax**

```
john --format=<hash-type> hash.txt --mask=<mask>
```

---

### **Full Brute Force**

```
john --format=<hash-type> hash.txt --mask=?a?a?a?a?a?a?a
```

---

### **Structured Mask Examples**

  

Uppercase first letter, three unknown characters, two digits:

```
john --format=<hash-type> hash.txt --mask=?u?a?a?a?d?d
```

Known prefix with unknown suffix:

```
john --format=<hash-type> hash.txt --mask=secret?a?a?a?a
```

---

## **Hybrid Mask Attacks (John)**

  

### **Mask + Wordlist**

  

Example: 123!Password

```
john --format=<hash-type> hash.txt --wordlist=dict.txt --mask=?a?a?a?a?w
```

### **Wordlist + Mask**

  

Example: Password123!

```
john --format=<hash-type> hash.txt --wordlist=dict.txt --mask=?w?a?a?a?a
```

---

## **John Custom Character Sets**

  

John supports **up to 9 custom charsets**:

```
-1 -2 -3 -4 -5 -6 -7 -8 -9
```

---

### **Restricted Starting Characters**

```
john --format=<hash-type> hash.txt \
-1=abcABC --mask=?1?a?a?a?a?d
```

---

### **Digits + Symbol Ending**

```
john --format=<hash-type> hash.txt \
-1=?u?l -2=!@$ --mask=?1?d?d?d?d?2
```

---

### **Multiple Custom Charsets**

```
john --format=<hash-type> hash.txt \
-1=qwer -2=poiu -3=123456 -4=!@#$% \
--mask=?2?2?1?4?3?3?1?1
```

---

## **Date-Based Masks**

  

Date patterns are among the **highest-yield structured masks** due to their strong ties to human memory, personal relevance, and policy constraints. These masks target common numeric constructions without exploding the keyspace.

  

Use these when:

- Birthdays, anniversaries, or years are likely
    
- Password policies allow digits-heavy suffixes/prefixes
    
- OSINT or contextual clues suggest dates
    

---

### **YYMMDD Date Mask**

  

Targets two-digit year formats commonly used for birthdays or anniversaries.

  

**Pattern Logic**

- YY → constrained decade values
    
- MM → month (01–12)
    
- DD → day (01–31, loosely bounded)
    

  

**Example Candidates**

```
900523
010912
851231
```

**Command**

```
hashcat -a 3 -m #type hash.txt -1 12 -2 90 -3 01 -4 123 ?1?2?3?d?4?d
```

---

### **YYYYMMDD Date Mask**

  

Targets full four-digit year formats, often seen in:

- Compliance-driven passwords
    
- Legacy enterprise environments
    
- “Strong-looking” but predictable constructions
    

  

**Pattern Logic**

- YYYY → constrained century values
    
- MM → month
    
- DD → day
    

  

**Example Candidates**

```
19901231
20030415
19870607
```

**Command**

```
hashcat -a 3 -m #type hash.txt -1 12 -2 90 -3 01 -4 123 ?1?2?d?d?3?d?4?d
```

---

## **Sequential Numbers + Special Character Mask**

  

Sequential digits remain extremely common due to ease of recall and perceived structure. Adding a trailing special character often satisfies policy without increasing entropy meaningfully.

  

**Pattern Logic**

- Digit progression by keypad column or row
    
- Trailing symbol to meet complexity requirements
    

  

**Example Candidates**

```
147!
258#
369$
```

**Command**

```
hashcat -a 3 -m #type hash.txt -1 147 -2 258 -3 369 ?1?2?3?s
```

---

## **Takeaway**

  

These masks demonstrate a core principle of effective mask design:

  

> **Human meaning beats mathematical randomness.**

  

Date formats and numeric sequences dramatically reduce the search space while maintaining high real-world coverage. When combined with policy awareness and contextual insight, these masks routinely outperform generic brute-force strategies.


[[Masks]]
[[Home]]