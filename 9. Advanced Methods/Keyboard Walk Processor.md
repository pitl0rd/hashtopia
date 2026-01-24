
# **Keyboard Walk Attacks (kwprocessor)**

  

Keyboard-walk attacks model a very specific and very human behavior:

**users dragging their fingers across adjacent keys to build passwords**.

Rather than guessing characters abstractly, keyboard-walk generators produce candidates based on **physical keyboard layout**, **directional movement**, and **adjacency constraints**.


This makes them especially useful for:

- PIN-like patterns
    
- Simple workstation passwords
    
- Legacy environments
    
- Users trained to “just type something quick”
    

---

## **What kwprocessor Does**

  

**kwprocessor (kwp)** generates password candidates by:

- Starting from a defined **base character set**
    
- Following **keyboard adjacency routes**
    
- Applying **directional movement rules**
    
- Emitting candidates via **stdout** for chaining into other tools
    

  

This is a **candidate generator**, not a cracker.

---

## **Why Keyboard Walks Matter**

  

Keyboard walks frequently appear as:

- `qwerty, asdf, zxcv
    
- `1qaz, 2wsx
    
- `qazwsx
    
- Diagonal or mirrored patterns
    
- Repeated directional motion
    

  

They are:

- Easy to remember
    
- Fast to type
    
- Common in weak or default password environments
    
- Underestimated in many cracking strategies

---

## **Core Components**

  

A kwprocessor run is built from three files:

|**Component**|**Purpose**|
|---|---|
|**Basechars**|Defines starting characters|
|**Keymap**|Describes keyboard layout and adjacency|
|**Routes**|Defines movement rules and walk depth|

---

## **Example: Minimal Keyboard Walk (Tiny Charset)**

  
**Tiny base charset**, English keyboard, **2–10 adjacent keys**, piped into Hashcat:

```
kwp.bin basechars/tiny.base keymaps/en.keymap routes/2-to-10-max-3 -0 -z | \
hashcat -a 0 -m #type hash.txt
```

**Why this works:**

- Focuses on short, high-probability walks
    
- Avoids excessive keyspace
    
- Ideal for early-phase attacks
    

---

## **Example: Tiny Charset with Explicit Piping**


Same attack, explicit pipeline:

```
kwp.bin basechars/tiny.base keymaps/en.keymap routes/2-to-10-max-3 -0 -z | \
hashcat -a 0 -m #type hash.txt
```

---

## **Example: Full Charset, Exhaustive 3×3 Walks**

Larger character set, tighter movement constraints:

```
./kwp basechars/full.base keymaps/en.keymap routes/3-to-3-exhaustive.route | \
hashcat -a 0 -m #type hash.txt
```

**Use this when:**

- You suspect keyboard-pattern complexity
    
- You want full coverage of short walk spaces
    
- Policy allows broader character sets
    

---

## **Directional Controls (Keywalk Logic)**

  

Keyboard movement is defined numerically:

|**Flag**|**Direction**|
|---|---|
|-1|South-West|
|-2|South|
|-3|South-East|
|-4|West|
|-5|Repeat key|
|-6|East|
|-7|North-West|
|-8|North|
|-9|North-East|
|-0|All directions|

These allow you to:

- Restrict to linear motion
    
- Exclude diagonals
    
- Model common hand movement habits
    

---

## **Distance Constraints**

  

Control how far keys may be apart:

|**Option**|**Meaning**|
|---|---|
|-n|Minimum distance|
|-x|Maximum distance|

Example:

- Tight distances = realistic finger movement
    
- Larger distances = more exploratory patterns
    

---

## **Keyboard Modifier Sets**

  

Control which characters are reachable:

|**Option**|**Description**|
|---|---|
|-b|Base (no shift/altgr)|
|-s|Shift characters|
|-a|AltGr characters|
|-z|Enable all modifiers|

Use **-b only** for highly conservative attacks.

Use **-z** for broader exploration.

---

## **Full Option Reference**

```
./kwp (options) ... basechars-file keymap-file routes-file

-V, --version           Print version
-h, --help              Print help
-o, --output-file       Output file

-b, --keyboard-basic    Base keys
-s, --keyboard-shift    Shift keys
-a, --keyboard-altgr    AltGr keys
-z, --keyboard-all      Enable all modifiers

-1..-9                  Directional controls
-0                      Enable all directions

-n                      Min key distance
-x                      Max key distance
```

---

## **When to Use Keyboard Walk Attacks**


Use kwprocessor when:

- Password policy is weak or unenforced
    
- Users are non-technical
    
- Default credentials are common
    
- You observe:
    
    - Short passwords
        
    - Low entropy
        
    - Repeated patterns
        
    - Spatial logic instead of linguistic logic
        
    

---

## **Strategic Placement in an Attack Plan**

  

Keyboard walks typically perform best:

1. **After** basic dictionary + rules
    
2. **Before** full brute force
    
3. **Alongside** masks targeting short lengths
    

  

They shine in:

- Early discovery
    
- Human-behavior analysis
    
- Pattern-driven research
    

---

## **Takeaways**

- Keyboard walks exploit **motor memory**, not creativity
    
- They generate **high-signal candidates** with minimal assumptions
    
- Small keyspaces often produce **outsized returns**
    
- They are ideal for **stdin/stdout chaining** and creative pipelines


---
[[Advanced Compositional Attacks]]
[[Home]]

  

#tools #sudad