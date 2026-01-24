
Salts and key stretching are mechanisms used to **modify how passwords are hashed**, with the goal of making large-scale guessing attacks more expensive and less efficient.

They do not make weak passwords strong.  They change the **economics and scalability** of attacks. Understanding this distinction is critical to accurately evaluating password security.

---

## What Is a Salt?

A **salt** is a non-secret value that is combined with a password before it is hashed.

In properly designed password storage systems, salts are typically:

- **Random**, so they cannot be guessed or predicted
    
- **Unique per password**, so each stored hash represents a distinct target
    
- **Stored alongside the hash**, since secrecy is not their purpose
    

When a salt is used, the same password entered by two different users will produce **completely different hash outputs**, even if the underlying password is identical. This property is fundamental to modern password storage design.

Salts do not make passwords stronger by themselves; instead, they change how passwords fail when systems are compromised.

---

## Why Salts Exist

Salts exist to address two systemic weaknesses that appear when passwords are hashed without additional context.

### 1. Identical Passwords

Without salts, identical passwords always produce identical hashes. This creates immediate problems in breached datasets.

For example:

- Attackers can instantly identify users who share the same password
    
- Cracking one hash automatically cracks every identical hash
    
- Password reuse across accounts becomes trivially visible
    

In such systems, effort scales _linearly_: one successful crack yields many compromised accounts. Salts disrupt this dynamic by ensuring that each password hash is treated as a **unique target**, even when multiple users choose the same password. An attacker must now crack each hash independently, dramatically increasing the total work required. From an analytical perspective, salts prevent password reuse from becoming an automatic multiplier inside a single dataset.

---

### 2. Precomputation Attacks

Without salts, attackers can prepare for breaches **in advance**.

This enables:

- Rainbow tables
    
- Precomputed hash dictionaries
    
- Large-scale reuse of cracking results across multiple breaches
    

In unsalted systems, attackers invest once and benefit repeatedly.

Salts eliminate this advantage by binding each hash to a random value that is not known until the database is obtained. As a result, attackers are forced to generate guesses **after** a breach occurs, recomputing hashes for every password–salt pair rather than relying on pre-built tables. This shifts attacks from a one-time investment into a per-breach, per-hash cost.

---

## What Salts Do _Not_ Solve

While salts are essential, they are not a complete defense.

Salts:

- Do **not** slow down guessing on their own
    
- Do **not** prevent weak or common passwords
    
- Do **not** stop offline attacks once hashes are exposed
    

If a hashing algorithm is fast (e.g., MD5, SHA-1, NTLM), attackers can still test billions of guesses per second, they simply have to do it separately for each salted hash.

This is why salts must be combined with **key stretching**, **iteration counts**, or **memory-hard functions** to meaningfully raise the cost of guessing.

---

## How Salts Change the Economics of Attacks

From a security perspective, salts transform password cracking from:

- **Many hashes, one solution**  
    
    into
    
- **Many hashes, many independent problems**
    

They prevent attackers from exploiting identical passwords and precomputed work, but they do not eliminate guessability. Instead, they ensure that guessing effort scales with the number of users rather than collapsing into a single shared failure.

---

## Salt Properties That Matter

Not all salts are equally effective. Key properties include:

- **Uniqueness**  
  Reusing salts across users reduces their effectiveness.

- **Randomness**  
  Predictable salts weaken protection.

- **Length**  
  Sufficient length prevents collisions and shortcutting.

- **Integration**  
  Salts must be properly incorporated into the hashing process not merely appended incorrectly.

The most important property is **per-password uniqueness**.

---

## What Is Key Stretching?

**Key stretching** is a defensive technique used in password hashing systems to deliberately increase the computational cost of each password guess. Rather than hashing a password once with a fast algorithm, key stretching makes each hash operation slower or more resource-intensive so that guessing passwords becomes significantly more expensive.

This increased cost is achieved by:

- Repeating the hash computation many times (iterations)
    
- Requiring large amounts of memory during computation
    
- Exposing tunable cost parameters that can be adjusted over time
    

From a defensive perspective, key stretching does not change what an attacker _can_ do, it changes **how much it costs them to do it**. Every additional unit of work per guess directly reduces the number of guesses an attacker can attempt per second.

---

## Why Key Stretching Exists

Modern CPUs, GPUs, and specialized hardware can compute **billions of fast hashes per second**. Algorithms like MD5, SHA-1, or SHA-256 were never designed to protect passwords against this scale of computation.

Key stretching exists to counter this imbalance.

By increasing the cost of each guess, key stretching:

- Slows down brute-force and dictionary attacks
    
- Reduces the advantage of GPUs and ASICs
    
- Forces attackers to invest more time, hardware, and money per target
    

The goal is not to make guessing impossible, that is unrealistic, but to make large-scale guessing **uneconomical**. A password that would fall instantly under a fast hash may take months or years to recover when proper key stretching is applied, changing the practical risk profile of a breach.

---

## Iterations vs Memory Hardness

There are two primary strategies for implementing key stretching, each addressing different aspects of modern attack capability.

---

### Iteration-Based Stretching

Iteration-based schemes increase cost by **repeating the same computation many times**. Each password guess must be hashed repeatedly before a result is produced.

Characteristics of iteration-based stretching include:

- Straightforward implementation
    
- Predictable and linear cost increases
    
- Compatibility with existing hash functions
    
- Limited resistance to massively parallel hardware
    

Examples include PBKDF2 and work-factor-based schemes such as bcrypt. While iterations significantly slow down CPUs, GPUs can still evaluate many guesses in parallel, which limits how effective iteration-only defenses are against well-resourced attackers.

---

### Memory-Hard Designs

Memory-hard designs increase cost by requiring **substantial memory usage** in addition to computation. Each password guess must allocate and repeatedly access large memory regions during hashing.

Characteristics of memory-hard designs include:

- Strong resistance to parallelization
    
- Reduced efficiency for GPUs and ASICs
    
- Higher cost per guess in both time _and_ memory
    
- Better alignment with modern threat models
    

Algorithms like scrypt and Argon2 intentionally trade raw speed for memory pressure, forcing attackers to either use far fewer parallel guesses or invest in expensive, memory-heavy hardware. In modern attack scenarios, **memory constraints are often more limiting than computation alone**.

---

## Parameter Selection and Trade-Offs

Key stretching is not inherently security, it is only as effective as the parameters chosen.

Every key-stretching scheme exposes tunable settings that control:

- How slow each hash operation is
    
- How much memory is required
    
- How much parallelism is allowed
    

Choosing these parameters involves balancing multiple competing factors:

- **Authentication latency** - users expect logins to feel instant
    
- **Server resource usage** - hashing occurs on live systems
    
- **Scalability** - parameters must hold up under peak load
    
- **Hardware evolution** - today’s safe settings may be weak tomorrow
    

Parameters that are set too low provide little real protection, while parameters that are set too high can cause performance degradation, denial-of-service risks, or poor user experience.

For this reason, key stretching should be treated as a **living control**, revisited and adjusted as hardware capabilities and threat models evolve. It is not a one-time configuration decision, but an ongoing defensive posture.

---

## How Salts and Key Stretching Interact

Salts and key stretching address different attack dimensions:

| Mechanism | Primary Benefit | Addresses |
|---------|------------------|----------|
| Salts | Attack isolation | Shared passwords, precomputation |
| Key stretching | Economic resistance | Guessing throughput |

Used together, they:
- Prevent attackers from sharing work
- Force per-hash investment
- Increase total cost at scale

---

## Common Misconceptions

- **“Salts make passwords secure.”**  
  Salts reduce attacker efficiency; they do not increase password strength.

- **“Any iteration count is enough.”**  
  Inadequate parameters can nullify the benefit.

- **“Key stretching is only needed for strong passwords.”**  
  Weak passwords benefit most from increased cost.

- **“Once configured, parameters never need review.”**  
  Hardware advances require periodic reassessment.

---

## Relationship to Other Concepts

Salts and key stretching interact directly with:

- **[[Cryptographic Hashing]]** → defines the hashing process being hardened
- **[[Entropy and Guessability]]** → determines how many guesses are needed
- **[[2. Cracking Methodology]]** → measures real-world impact of parameters
- **[[Scale and Risk Amplification]]** → shows how small inefficiencies compound

These mechanisms only matter in the context of system-wide behavior.

---

## Intended Outcome

After understanding this concept, readers should:

- Know what salting and key stretching actually protect against
- Understand why weak passwords still fail despite modern hashing
- Recognize the importance of parameter selection
- Avoid overestimating the security benefit of hashing alone


[[1. Concepts|Concepts]]
[[Home]]
#research #education 
