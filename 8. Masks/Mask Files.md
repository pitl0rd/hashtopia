
Mask files provide a **structured, repeatable way** to run large sets of related mask attacks without manually invoking each mask on the command line. Rather than treating masks as one-off guesses, mask files allow you to **encode behavioral assumptions at scale**.

---

## **What Is a Mask File?**

  

A **mask file** is a plain text file containing:

- **One mask per line**
    
- Saved with the .hcmask extension
    
- Loaded by Hashcat using the -m attack mode -a 3 (mask attack)
    

  

Each line is treated as an independent mask attack, executed in sequence.

  

This makes mask files ideal for:

- Replaying known human password patterns
    
- Running region-specific or policy-specific attacks
    
- Encapsulating research findings into reusable artifacts
    

---

## **Why Mask Files Matter**

  

Mask files shift your workflow from:

  

> “Try this mask and see what happens”

  

to:

  

> “Execute a curated model of human password behavior”

  

Key advantages:

- **Repeatability** – Same masks, same order, same results
    
- **Comparability** – Easy to benchmark datasets or policies
    
- **Scalability** – Hundreds or thousands of masks in a single run
    
- **Documentation** – Masks become explicit assumptions, not tribal knowledge
    

---

## **Using a Mask File in Hashcat**

  

Basic usage:

```
hashcat -a 3 -m <hash_type> hashes.txt masks.hcmask
```

Optional flags (commonly paired):

- -i – incremental mask expansion
    
- --runtime – cap execution time
    
- --session – allow pause/resume
    
- --restore – resume interrupted runs
    

---

## **Hashcat Built-In Mask Files**

  

Hashcat ships with several **prebuilt mask files**, primarily derived from large real-world password datasets (notably RockYou).

  

These are designed to cover **high-yield human patterns first**, before expanding into broader keyspace.

  

### **Included Mask Files**

|**Mask File Name**|**Approx. Masks**|**Purpose**|
|---|---|---|
|8char-11-1u-1d-1s-noncompliant.hcmask|24,712|Targets common 8-character passwords with minimal complexity|
|rockyou-1-60.hcmask|836|Very high-frequency RockYou patterns|
|rockyou-2-1800.hcmask|2,968|Slightly expanded RockYou structures|
|rockyou-3-3600.hcmask|3,971|Mid-range human patterns|
|rockyou-4-43200.hcmask|7,735|Broader structural variation|
|rockyou-5-86400.hcmask|10,613|Long-tail RockYou patterns|
|rockyou-6-864000.hcmask|17,437|Aggressive structural coverage|
|rockyou-7-2592000.hcmask|25,043|Very broad, diminishing-return masks|

---

## **Interpreting These Mask Sets**

  

These mask files are **not brute force** in the traditional sense.

  

They are:

- Ordered approximations of **human password construction**
    
- Designed to front-load **high-probability structures**
    
- Intended to expose **policy weaknesses** and **user behavior patterns**
    

  

Important takeaway:

  

> Even large mask files still represent a **tiny fraction** of total keyspace, yet recover a disproportionate number of passwords.

---

## **When to Use Mask Files**

  

Mask files are especially effective when:

- Password policies are weak or loosely enforced
    
- User behavior is homogeneous (shared culture, region, role)
    
- You want **explainable results** instead of raw throughput
    
- You are comparing **policy impact**, not chasing maximum recovery
    

  

They are less effective when:

- Passwords are truly random
    
- Password managers dominate
    
- Long, unique secrets are enforced and audited
    

---

Using mask files effectively means:

- Understanding _why_ a mask exists
    
- Knowing _what behavior_ it models
    
- Recognizing _when results reflect policy failure, not tooling success_
    


---
[[Masks]]
[[Home]]
#hashcat_attacks 
#masks 