
Brute-force rules allow you to **systematically expand dictionary candidates** by applying every possible transformation defined by a rule set. When combined with a wordlist, this enables **Hybrid attacks** without enumerating a full brute-force keyspace.

Instead of cracking:

```
??????????
```

You crack:

```
dictionary_word + brute_force_space
```

This dramatically reduces runtime while still covering common user behavior.

Brute-force rules are generated using **maskprocessor** and consumed by **Hashcat** via the `-r` flag.

---

## Concept: Brute-Force Rules

A _brute-force rule_ is a rule file that contains **every possible mutation** for a constrained character space.

Think of it as:

- Mask attack logic
    
- Expressed as rules
    
- Applied to a dictionary
    

This turns a dictionary attack into a **controlled hybrid attack**.

---

## Tooling

- **maskprocessor** – rule generation
    
- **hashcat** – rule execution
    

---

## Appending to Dictionary (Most Common)

### Use Case

Users commonly append:

- Years (`1980–2025`)
    
- Digits (`1`, `123`, `0000`)
    
- PIN-style suffixes
    

This method appends numeric brute-force space to every dictionary word.

### Example: Append `0000 → 9999`

```bash
./mp64.bin -o bf.rule '$?d $?d $?d $?d'
```

**What this generates**

- `$0`
    
- `$1`
    
- `$2`
    
- …
    
- `$9999`
    

Each rule appends four digits to the base word.

### Example Hashcat Usage

```bash
hashcat -a 0 -m <hash_type> hashes.txt wordlist.txt -r bf.rule
```

**Effective Coverage**

```
password0000
password0001
password0002
...
```

---

## Prepending to Dictionary

### Use Case

Less common, but still observed:

- Initials
    
- Short prefixes
    
- Region or org tags
    

This prepends brute-force characters before dictionary entries.

### Example: Prepend `aa → zz`

```bash
./mp64.bin -o bf.rule '^?l ^?l'
```

**What this generates**

```
aapassword
abpassword
...
zzpassword
```

### Example Hashcat Usage

```bash
hashcat -a 0 -m <hash_type> hashes.txt wordlist.txt -r bf.rule
```

---

## When to Use Brute-Force Rules

|Scenario|Recommended Approach|
|---|---|
|Known base words|Rules|
|Known suffix patterns|Brute-force rules|
|Unknown structure|Mask|
|Word + digits|Hybrid via rules|
|Policy-driven passwords|Rules > Mask|

---

## Brute-Force Rules vs Mask Attacks

|Feature|Brute-Force Rules|Mask Attack|
|---|---|---|
|Requires wordlist|Yes|No|
|Human-behavior aware|Yes|Limited|
|GPU efficiency|High|High|
|Keyspace control|Medium|Precise|
|Best for|Real users|Unknown users|

**Rule of thumb:**

> If you have _words_, use **rules**.  
> If you have _patterns only_, use **masks**.

---

## Performance Notes

- Rule files grow **exponentially**, keep brute spaces small.
    
- Prefer:
    
    - 2–4 digits
        
    - Limited charsets
        
- Test rule output first:
    

```bash
hashcat wordlist.txt -r bf.rule --stdout
```

---

## Common Extensions

- Append digits + symbols
    
- Prepend initials + digits
    
- Chain brute-force rules with curated rulesets (e.g. best64)
    

Example:

```bash
hashcat -a 0 -m <type> hashes.txt wordlist.txt \
  -r bf.rule \
  -r best64.rule
```

---

## Hashtopia Takeaway

Brute-force rules are a **precision amplifier**:

- Smaller than brute-force
    
- Smarter than masks
    
- Faster than full hybrids
    
[[Rules]]
[[Home]]
#bruteforce #wordlists #rules 
