

Masks define **positional character constraints** for password candidate generation.

Each position in a mask represents **one character slot**, and each placeholder expands to a defined character set.

  

Masks are most effective when:

- Password **length is known or tightly bounded**
    
- Character placement follows **human patterns**
    
- Dictionary and rule attacks have plateaued
    

---

## **John the Ripper - Mask Reference**

  

John’s mask engine is flexible and supports **extended character classes**, including non-ASCII ranges and hybrid placeholders.

  Use with [[Concept Application - John the Ripper]]

### **Core Mask Tokens**

|**Token**|**Description**|
|---|---|
|?l|Lowercase letters abcdefghijklmnopqrstuvwxyz|
|?u|Uppercase letters ABCDEFGHIJKLMNOPQRSTUVWXYZ|
|?d|Digits 0123456789|
|?s|Specials (space + punctuation)|
|?a|All characters (?l?u?d?s)|
|?h|8-bit characters (0x80–0xff)|
|?A|All valid characters in the current code page|
|?H|All characters except NULL|
|?L|Non-ASCII lowercase letters|
|?U|Non-ASCII uppercase letters|
|?D|Non-ASCII digits|
|?S|Non-ASCII specials|
|?w|**Hybrid placeholder** (original word when combining wordlist + mask)|

---

### **John Example Masks**

```
# 8 lowercase letters
john --mask=?l?l?l?l?l?l?l?l

# Capitalized word + 2 digits
john --mask=?u?l?l?l?l?d?d

# Wordlist + 4 unknown chars
john --wordlist=dict.txt --mask=?w?a?a?a?a
```

---

## **Hashcat - Mask Reference**

  Use with [[Concept Application - HashCat]]

Hashcat’s mask engine is **GPU-optimized**, deterministic, and strictly defined.

Character sets are smaller but faster and more predictable.

  

### **Core Mask Tokens**

|**Token**|**Description**|
|---|---|
|?l|Lowercase (26) abcdefghijklmnopqrstuvwxyz|
|?u|Uppercase (26) ABCDEFGHIJKLMNOPQRSTUVWXYZ|
|?d|Digits (10) 0123456789|
|?s|Specials (33 characters)|
|?a|All printable ASCII (95 characters)|
|?h|Hex lowercase 0123456789abcdef|
|?H|Hex uppercase 0123456789ABCDEF|
|?b|Byte 0x00–0xff (binary range)|

---

### **Hashcat Example Masks**

```
# 6 digits
hashcat -a 3 -m #type hash.txt ?d?d?d?d?d?d

# Capitalized word + 2 digits
hashcat -a 3 -m #type hash.txt ?u?l?l?l?l?d?d

# Known prefix + unknown suffix
hashcat -a 3 -m #type hash.txt secret?a?a?a?a
```

---

## **Custom Charset Buffers (Both Tools)**

  

Custom buffers drastically reduce keyspace by constraining **specific positions**.

  

### **Defining Custom Sets**

|**Flag**|**Purpose**|
|---|---|
|-1|Custom charset 1|
|-2|Custom charset 2|
|-3|Custom charset 3|
|-4|Custom charset 4|

---

### **Example - Targeted Custom Mask**

```
# Password starts with A/B/C (upper/lower), ends in digit
hashcat -a 3 -m #type hash.txt -1 abcABC ?1?a?a?a?a?d
```

```
# Upper/lower letter + 4 digits + special
hashcat -a 3 -m #type hash.txt -1 ?u?l -2 !@$ ?1?d?d?d?d?2
```

---

## **Conceptual Takeaways**

- **Masks model structure**, not creativity
    
- They excel at recovering:
    
    - Dates
        
    - Numeric suffixes
        
    - Capitalization habits
        
    - Fixed prefixes/suffixes

[[Masks]]
[[Home]]
#masks #reference