

  

_Exploratory pipelines using stdin / stdout_

  

This section describes **experimental password-generation and analysis pipelines** that rely on standard UNIX input/output streams (stdin / stdout) to connect multiple tools into a single, adaptive workflow.

These pipelines are **not intended to represent optimal or recommended attacks**. Instead, they exist to illustrate how password analysis can evolve beyond isolated tools and static command execution.

Specifically, these experiments are designed to demonstrate:

- How password candidate generation can be **modular and composable**
    
- How generators, filters, and transformers can be chained together
    
- How candidate spaces can be **reshaped dynamically** based on intermediate output
    
- How cracking engines can consume streams rather than fixed files

---

## **Why Composition Matters**

  

Most password tooling assumes a static workflow:

- Prepare a wordlist
    
- Apply rules or masks
    
- Run a cracker
    
- Review results
    

While effective, this approach locks assumptions in early. Any change requires restarting or duplicating work.


Compositional pipelines allow analysts to:

- Delay decisions until more data is available
    
- Insert analysis or filtering stages mid-stream
    
- Reuse outputs as inputs without intermediate files
    
- Experiment with structure, ordering, and transformation interactively
    

This is especially valuable in research contexts where the goal is **understanding behavior**, not simply maximizing crack rates.

---

## **Design Philosophy**

  

Traditional cracking workflows are usually linear:

  

> _Generator → Cracker → Results_

  

In contrast, compositional workflows treat password analysis as a **flow of transformations**, not a single execution step.

  

These experiments intentionally allow:

- **Multiple generators in sequence**
    
    (e.g., statistical generator → structural generator → mutation engine)
    
- **Feedback-like transformations**
    
    (e.g., cracked outputs informing downstream candidate shaping)
    
- **Structural reshaping before cracking**
    
    (e.g., filtering by length, character class, or pattern before evaluation)
    
- **Late binding of rules and constraints**
    
    (rules applied after generation instead of baked into the generator)

---

## **PRINCE → MDXFIND Pipeline**

  

### **Concept**

  

Use **PRINCE** to generate compositional candidates, then immediately filter or correlate them against a target hash set using mdxfind.

  

### **Example**

```
pp64.bin dict.txt | mdxfind -h ALL -f hash.txt -i 10 stdin > out.txt
```

### **What’s happening**

- pp64.bin generates chained candidates from dict.txt
    
- Output is streamed directly into mdxfind
    
- mdxfind performs fast lookup / matching against known hashes
    
- No intermediate files are required
    

  

### **Why this matters**

  

This demonstrates **early elimination**, discarding low-value candidates _before_ they ever reach a cracking engine.

---

## **Combinator → PRINCE → Hashcat**
#hashcat_attacks #combinator 
  

### **Concept**

  

Pre-expand the base vocabulary using combinators, then allow PRINCE to build higher-order compositions, and finally apply rules during cracking.

  

### **Two-dictionary combinator**

```
combinator.bin dict.txt dict.txt | pp64.bin | hashcat -a 0 -m hash_type hash.txt -r best64.rule
```

### **Three-dictionary combinator**

```
combinator3.bin dict.txt dict.txt dict.txt | pp64.bin | hashcat -a 0 -m hash_type hash.txt -r rockyou-30000.rule
```

### **What’s happening**

- Combinators increase **semantic density**
    
- PRINCE re-chains those combinations into longer structures
    
- Rules are applied _late_, inside the cracker
    

  

### **Insight**

  

This pipeline separates **semantic expansion** from **syntactic mutation**, making each stage easier to reason about.

---

## **Hashcat STDOUT → PRINCE → Hashcat**
#hashcat_attacks 
  

### **Concept**

  

Use Hashcat itself as a **candidate generator**, then reshape that stream with PRINCE, and finally crack.

  

### **Dictionary + rules → PRINCE**

```
hashcat -a 0 dict.txt -r dive.rule --stdout | pp64.bin | hashcat -a 0 -m hash_type hash.txt
```

### **Dictionary + hybrid mask → PRINCE**

```
hashcat -a 6 dict.txt ?a?a?a?a --stdout | pp64.bin --pw-min=8 | hashcat -a 0 -m hash_type hash.txt
```

### **Mask + dictionary → PRINCE**

```
hashcat -a 7 ?a?a?a?a dict.txt --stdout | pp64.bin --pw-min=8 | hashcat -a 0 -m hash_type hash.txt
```

### **Why this is interesting**

- Hashcat is no longer “the final step”
    
- It becomes a **programmable generator**
    
- PRINCE reshapes candidates _after_ rules and masks
    

  

This inverts the usual hierarchy of tools.

---

## **Maskfile → PRINCE → Hashcat**
#hashcat_attacks 
  

### **Concept**

  

Use probabilistic mask files as a structured generator, then let PRINCE recombine those outputs before cracking.

  

### **Maskfile on the left**

```
hashcat -a 6 dict.txt rockyou-1-60.hcmask --stdout | pp64.bin --pw-min=8 --pw-max=14 | hashcat -a 0 -m hash_type hash.txt
```

### **Maskfile on the right**

```
hashcat -a 7 rockyou-1-60.hcmask dict.txt --stdout | pp64.bin --pw-min=8 --pw-max=14 | hashcat -a 0 -m hash_type hash.txt
```

### **Structural takeaway**

  

Mask files encode **distributional assumptions**.

PRINCE then recombines those assumptions into higher-order patterns.

---

## **Why These Matter (Even If You Never Use Them)**

  

These pipelines are valuable because they:

- Reveal hidden assumptions in “standard” attacks
    
- Show how tools can be repurposed
    
- Make candidate generation **observable and inspectable**
    
- Demonstrate that cracking logic is modular

They are best understood as **research instruments**, not recipes.

---

## **Takeaways**

- stdin / stdout turns tools into **building blocks**. This is an incredibly powerful concept to master
    
- Generators, transformers, and crackers can be reordered
    
- Candidate spaces are malleable, not fixed
    
- Creativity often reveals more than optimization
    
- Understanding _why_ something works matters more than _how fast_ it runs
    

> Go forth and create something incredible.

---

[[Home]]

#advanced #sudad 