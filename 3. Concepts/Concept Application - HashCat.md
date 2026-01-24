
This guide is intended as an introduction to a **general password cracking workflow** for educational and defensive purposes.

We’ll assume that you are comfortable running applications from the command line and that you are working with **NTLM (1000)** password hashes. In this scenario, the hashes represent user passwords stored in plain text form before hashing.

NTLM is considered a **fast hash**: it takes relatively little computational effort to compute each hash. From an analytical perspective, this means:

- We can test a **large number of candidate passwords** (a larger keyspace)
    
- We do not pay as high a time penalty per guess
    
- NTLM-based datasets are useful for demonstrating and studying cracking workflows
    

Because the underlying passwords in this example are plain text words and phrases, starting with a **wordlist/dictionary-based attack** is a reasonable first step. If the data being hashed followed a different pattern (machine-generated strings, high-entropy secrets, or structured tokens), a different starting strategy would make more sense.

In the examples below, `HashCat` is used as the primary tool, but the overall **methodology** applies to any password cracking framework with similar capabilities.

At each stage of the workflow, new information will emerge, such as common patterns, recurring words, or frequent structures. A key goal of this guide is to emphasize that you should **continuously adapt your approach** based on what you observe, refining your attack plan to become more focused, efficient, and informed by real data.

If you need to obtain **local Windows password hashes**, the methods described in the  
**[[Windows Local Password Hashes]]** section provide several reliable approaches for retrieving NTLM credentials from standalone systems.

For environments using Active Directory, extracting **domain password hashes** requires different techniques. You can explore multiple collection strategies in the  
**[[Windows Domain Password Hashes]]** section.

If you already have NTLM hashes and simply need to prepare or reformat them for processing, the  **[[Processing]]** page offers guidance on structure, normalization, and expected formats for cracking tools.

If your goal is to learn, practice, or benchmark cracking techniques, you can use curated hash sets arranged by increasing complexity in **[[Example Hashes]]**.  

You can also generate your own test hashes through a variety of freely available tools.

Once your dataset is ready, follow the workflow below in order. It outlines a high-level, educational cracking strategy designed to help you reason through each stage, understand why certain decisions matter, and build progressively more efficient attack plans.

## Custom Wordlist Attack  
#wordlists #hashcat_attacks

Start by compiling all **known or suspected plaintext passwords** into a  
[[Wordlist Engineering & Manipulation|custom wordlist]] file. This captures environment-specific patterns (company name, local jargon, seasonal favorites, etc.) and should be your most targeted dataset.

Use this custom list in a straight dictionary attack:

```bash
hashcat -a 0 -m 1000 -w 4 hash.txt custom_list.txt
```

## Custom Wordlist Attack + Rules

#wordlists #hashcat_attacks

Once you’ve tried exact matches from your [[Wordlist Engineering & Manipulation|custom wordlist]], the next step is to look for small variations of those passwords. This is where [[Rules|rules]] come in: they apply transformations (add digits, symbols, change case, etc.) to your known candidates.

Run your custom wordlist with a rules-based dictionary attack:
```bash
hashcat -a 0 -m 1000 -w 4 hash.txt custom_list.txt -r best64.rule --loopback
```

`--loopback` allows Hashcat to reuse newly cracked passwords as additional input, amplifying coverage around successful patterns.

## Wordlist Attacks

#wordlists #hashcat_attacks

After exhausting your custom list, widen your search to public wordlists and breach-based dictionaries. These contain very common passwords and real-world leaked data.

Perform a broad dictionary attack using well-known [[Wordlists and Dictionaries|dictionaries/wordlists]]:
```bash
hashcat -a 0 -m 1000 -w 4 hash.txt dict.txt
```

This step often recovers a large portion of weak and [[Password Reuse|reused passwords]].
## Wordlist Attacks + Rules

#wordlists #hashcat_attacks

Next, combine your broad dictionaries with transformation [[Rules|rules]] to capture slightly modified forms of common passwords (e.g., Password1!, Summer2024, Welcome@123).

Run a rule-augmented dictionary attack:
```bash
hashcat -a 0 -m 1000 -w 4 hash.txt dict.txt -r best64.rule --loopback
```

At this stage, you are exploring realistic human variations on familiar password themes.
## Fingerprint Custom Wordlist + Rules

#wordlists #hashcat_attacks #awk

As you crack more passwords, you gain a better fingerprint of the environment’s behavior. Add these newly discovered passwords back into your [[Custom Dictionary Generation|custom wordlist]] so future attacks reflect real user choices.

Extract cracked passwords from the hashcat.potfile:
```bash
awk -F ":" '{print $2}' hashcat.potfile >> custom_list.txt
```
Then rerun a focused rules-based attack against this enriched list:
```bash
hashcat -a 0 -m 1000 -w 4 hash.txt custom_list.txt -r dive.rule --loopback
```

This step iteratively refines your understanding of the password population.
## Mask Attacks

#wordlists #hashcat_attacks

Once high-yield dictionary and rule-based attacks have been run, you can use [[Mask Attacks -Debugging, Construction, and Customization|mask attacks]] to systematically explore common length and composition patterns.

Hashcat ships with mask files based on real-world data such as [[Wordlists and Dictionaries#Rockyou.txt|RockYou]]. These masks target structures that occur frequently in leaked passwords.

Example:
```bash
hashcat -a 3 -m 1000 -w 4 hash.txt rockyou-1-60.hcmask
```
This step moves from word-based guessing into structured keyspace exploration as we learned in the [[Password Structure and Composition]] section.
## Hybrid Wordlist + Masks

#wordlists #hashcat_attacks

Hybrid attacks combine the strengths of dictionary attacks and mask attacks. You take real words or known patterns and append or prepend structured patterns (digits, years, symbols, etc.).

Using a dictionary of your choice, run hybrid attacks:

## Dictionary + mask (append)
```bash
hashcat -a 6 -m 1000 -w 4 hash.txt dict.txt rockyou-1-60.hcmask
```

## Mask + dictionary (prepend)
```bash
hashcat -a 7 -m 1000 -w 4 hash.txt rockyou-1-60.hcmask dict.txt
```

This helps uncover passwords like `Summer2024!, !Finance123, or Admin#2023`.
## Update Fingerprint Custom Wordlist + Rules

#wordlists #hashcat_attacks #awk

Continue feeding your discoveries back into your custom wordlist. Each new cracked password improves your picture of user behavior and makes subsequent attacks more targeted.

Update your custom list:
```bash
awk -F ":" '{print $2}' hashcat.potfile >> custom_list.txt
```

Then re-run a focused, high-variation ruleset:
```bash
hashcat -a 0 -m 1000 -w 4 hash.txt custom_list.txt -r dive.rule --loopback
```

This creates a feedback loop: more cracks → better wordlist → more cracks.
## Combinator Attacks

#wordlists #hashcat_attacks

Next, use a combinator attack to combine pairs of dictionary entries into longer, multi-part passwords. This models users who join two meaningful strings (names, words, phrases).

Example:
```bash
hashcat -a 1 -m 1000 -w 4 hash.txt dict.txt dict.txt
```

This is useful for capturing constructions like `SummerVacation, AdminPortal, or BlueDragon`.
## Update Fingerprint Custom Wordlist + Hybrid

#wordlists #hashcat_attacks #awk

Again, incorporate newly cracked passwords back into your environment-specific custom wordlist, then leverage hybrid attacks to explore further variations around them.
```bash
awk -F ":" '{print $2}' hashcat.potfile >> custom_list.txt
```

Then run hybrid attacks using these refined candidates:

## Append masks to custom list
```bash
hashcat -a 6 -m 1000 -w 4 hash.txt custom_list.txt rockyou-1-60.hcmask
```

## Prepend masks to custom list
```
hashcat -a 7 -m 1000 -w 4 hash.txt rockyou-1-60.hcmask custom_list.txt
```
At this stage you are heavily exploiting observed local behavior.
## Custom Mask Attacks

#wordlists #hashcat_attacks

By now, many of the easier passwords should be cracked. The remaining ones often have less common structures, but those structures still follow patterns.

Using [[PACK - Password Analysis and Cracking Kit|P.A.C.K.]], derive custom mask attacks from your cracked passwords. This surfaces statistically common patterns specific to your dataset.

Generate and then run your custom masks:
```bash
hashcat -a 3 -m 1000 -w 4 hash.txt custom_masks.hcmask
```

Be sure to remove masks that duplicate those from the previous rockyou-1-60.hcmask list to avoid wasted effort.
## Brute Force Attacks

#bruteforce #hashcat_attacks

As a last resort, you can fall back to pure brute force. This means exploring all combinations within a given keyspace, without assumptions about words, patterns, or structure.

Because keyspace grows exponentially with length, you must be selective:

`Short lengths (e.g., up to 7–8 characters) may be feasible

`Beyond ~10 characters, brute force becomes impractical on typical hardware

Example:
```bash
hashcat -a 3 -m 1000 -w 4 hash.txt -i ?a?a?a?a?a?a?a?a
```

The goal here is to capture any remaining weak, short, fully non-lexical passwords. This is the last resort for the creative analyst.

#sudad
[[1. Concepts|Concepts]]
[[Home]]