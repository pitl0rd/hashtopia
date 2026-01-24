
# Character Frequency CLI Tool (`characterfreq.py` / `charfreq.py`)

## Purpose

Analyze a plaintext password list and summarize **character frequency**. Useful for quickly spotting dominant characters (e.g., digits-heavy corpora, vowel bias, special-char habits) and validating assumptions before building masks, rules, or PCFG training sets.

## Best For

- Pre-attack **dataset profiling** (defensive research / authorized assessments)
    
- Understanding **charset bias** for mask design
    
- Comparing corpora (e.g., internal vs public breach distributions)
    

## Inputs / Outputs

- **Input:** `passwords.txt` (one password per line)
    
- **Output:** summary + (optionally) a frequency plot
    

## Basic Usage

```bash
charfreq.py <options> passwords.txt
```

## Options (as provided)

- `-w` - window size to analyze (default: 1)
    
- `-r` - rolling window size
    
- `-s` - skip spaces, tabs, newlines
    

## Operational Notes

- Treat password lists as **sensitive** even if “just plaintext samples.”
    
- If you’re generating plots for reporting: consider aggregating + redacting rather than preserving raw inputs.
    

## Common Pitfalls

- Mixed encodings can skew counts (UTF-8 vs Latin-1 artifacts).
    
- If the input contains usernames/emails mixed in, your frequency results become “identity-string frequency,” not password frequency.
    

## Tags

`#tools` `#analysis` `#password-research` `#character-frequency`

---

# hashID

## Purpose

Identify likely hash algorithms using regex matching. Can optionally print corresponding **hashcat mode** and/or **John the Ripper format** mappings. HashID is a great tool that you can use in conjunction with the methods described in the [[Concept Application - HashCat]]and [[Concept Application - John the Ripper]] sections.

## Best For

- Triage: “What is this hash string?”
    
- Sorting mixed hash dumps into cracking pipelines
    
- Quickly mapping to tool-specific formats/modes (hashcat/JtR)
    

## Inputs / Outputs

- **Input:** a single hash, `STDIN`, or a file of hashes
    
- **Output:** list of likely hash types (sometimes multiple candidates)
    

## Install

```bash
pip install hashid
```

## Usage

```bash
hashid INPUT
```

## Common Options

- `-e, --extended` - include more algorithms (including salted candidates)
    
- `-m, --mode` - show hashcat mode
    
- `-j, --john` - show JtR format
    
- `-o FILE, --outfile FILE` - write output to a file
    

## Examples

```bash
hashid '$P$8ohUJ.1sdFw09/bMaAQPTGDNi2BIUt1'
hashid -mj '$racf$*AAAAAAAA*3c44ee7f409c9a9b'
hashid hashes.txt
```

## Operational Notes

- hashID is **heuristic**. Some formats overlap; always validate with context:
    
    - source app/platform
        
    - field length constraints
        
    - presence/structure of salts
        
- For high confidence, pair with: sample verification, app code/config, or known hash examples.
    

## Tags

#tools #hash-id #triage #sudad 

---

# NTLM Hash Generator (Web)

## Purpose

Generate LM/NTLM hashes for **testing and verification** (e.g., validating parsing, building unit tests, training demos).

## Tool

- Web generator: `https://tobtu.com/lmntlm.php`
    

## Best For

- Quick lab validation (format checks, known-answer tests)
    
- Demos (showing LM vs NTLM differences)
    

## Caution

- Don’t paste real production passwords into third-party sites.
    
- Prefer offline tooling for sensitive inputs.
    

## Tags

#tools #ntlm #testing

---

# pigz

## Purpose

A parallel implementation of gzip. Speeds up compression/decompression on multi-core systems very useful for large wordlists, corpora, and crack results.

## Best For

- Compressing huge wordlists/dumps faster
    
- Pipeline workflows where gzip becomes a bottleneck
    

## Examples

```bash
pigz -9 wordlist.txt
pigz -d wordlist.txt.gz
```

## Notes

- You trade CPU for time (by design).
    
- Great for speeding up archival + transport of large corpora.
    

## Tags

#tools #performance #compression

---

# Terminal Command-Line Shortcuts (Reference)

## Purpose

Quality-of-life keybinds when living in shells during analysis pipelines. This is by no means a comprehensive guide and committing these to memory will save you a lot of time over the course of your analysis.

## Shortcuts

- `Ctrl + u` - delete from cursor to start of line
    
- `Ctrl + w` - delete previous word
    
- `Ctrl + l` - clear screen
    
- `Ctrl + a` - jump to beginning of line
    
- `Ctrl + e` - jump to end of line
    
- `Ctrl + r` - reverse search command history (Esc to exit)

[[Resources]]
[[Home]]
#reference #terminal
