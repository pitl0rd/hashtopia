
Cryptographic hashing is one of the foundational concepts in password security.  
It describes how systems transform passwords into fixed-length outputs that can be stored, compared, and verified **without exposing the original password**.

Hashing is not encryption, and it is not intended to be reversible.  
Understanding what hashing does and does *not* do is essential for accurately measuring password risk.

---

## What Is a Cryptographic Hash?

A cryptographic hash function is a deterministic algorithm that transforms an input (such as a password) into a fixed-length output called a **hash** or **digest**.

A secure hash function exhibits:

- **Determinism**  
  The same input always produces the same output.

- **Preimage resistance**  
  It should be computationally infeasible to derive the original input from the hash.

- **Second preimage resistance**  
  It should be infeasible to find another input that produces the same hash.

- **Collision resistance**  
  Two different inputs should not produce the same output.

---

## What Hashing Does *Not* Do

A common misunderstanding is that hashing protects passwords from guessing.  
It does not.

Hashing **does not** prevent:

- Weak passwords from being guessed  
- Attackers from trying large numbers of inputs  
- Users from reusing simple or predictable structures  
- Passwords from being cracked if hashing is implemented poorly

Hashing only prevents attackers from reading passwords directly if the database is exposed. It does not make passwords *strong*, it makes them *unrecoverable by design*.

---

## Why Password Hashing Exists

Systems need a way to check whether a user typed the correct password **without storing the password itself**.

Hashing enables this workflow:

1. User enters a password  
2. System hashes the password  
3. System compares the hash with the stored hash  
4. If they match, authentication succeeds

The system never needs the original password again, which significantly reduces risk by ensuring that even if a password database is exposed, attackers do not immediately gain access to usable credentials and must instead invest time and resources attempting to recover the original passwords through guessing and analysis.

---

## Hash Formats and Representations

Hashes are commonly represented in:

- Hexadecimal  
- Base64  
- Modular crypt format (e.g., `$id$salt$hash`)  
- Algorithm-specific encodings (e.g., bcrypt)

A hash *format* is distinct from the hash *function*.  Understanding the format helps determine:

- Which algorithm was used  
- Whether a salt or iteration count is present  
- Whether the hash is modern or legacy

Correctly identifying a hash is a prerequisite for meaningful analysis, because the format reveals critical information about the underlying algorithm, the presence of salts or iteration counts, and the defensive assumptions made by the system, all of which directly influence how the hash can be evaluated, tested, and interpreted.

---

## The Role of Salting

A **[[Salts and Key Stretching|salt]]** is a random value added to a password before hashing.

Purposes of salting:

- Ensure identical passwords do not produce identical hashes  
- Prevent use of precomputed lookup tables (rainbow tables)  
- Force attackers to perform per-hash work rather than per-password work

Salts do **not** make passwords stronger, they make attacks slower and more expensive. When implemented correctly, they can add a vital additional layer of security.

---

## Iterations, Key Stretching, and Memory-Hard Functions

Some hashing schemes apply the hash function repeatedly (iterations) or use memory-hard designs to increase computational cost.

Examples include:

- PBKDF2 (iteration-based)  
- bcrypt (work factor)  
- scrypt (memory-hard)  
- Argon2 (memory + time cost)

Key stretching turns a fast function into a deliberately **slow** one, making password guessing more expensive.

The effectiveness of these schemes depends on:

- Parameter choices  
- Hardware trends  
- Attacker capabilities  
- System performance constraints

Imagine two systems that both store user passwords using SHA-256.

**System A** hashes each password **once** using plain SHA-256.  
**System B** applies SHA-256 through PBKDF2 with **200,000 iterations**.

From the user’s perspective, both systems behave the same: they enter a password and log in.  
From an attacker’s perspective, the difference is enormous.

If an attacker can test **25 billion SHA-256 hashes per second** (Yes, RTX 4090's have been clocked doing this) on modern hardware, System A allows billions of guesses every second. A leaked hash database could be exhaustively tested against common passwords in minutes or hours.

System B, however, forces the attacker to perform **200,000 hash operations per guess**. That same hardware can now test only a few thousand guesses per second. A password that would fall almost instantly in System A may take weeks, months, or longer to recover in System B.

Now consider a **memory-hard function** like scrypt or Argon2. These schemes not only repeat the computation, but also require large amounts of memory for each guess. This sharply reduces the advantage of GPUs and specialized hardware, forcing attackers to trade speed for memory and making large-scale attacks significantly more expensive.

This example highlights why **parameter choices matter**. Iteration counts, memory requirements, and time costs directly determine how much effort an attacker must expend per guess,  and whether an attack is practical at all.


---

## Fast vs. Slow Hashing

**Fast hash functions** (MD5, SHA-1, SHA-256) were not designed for password storage.  
Their speed makes them vulnerable to large-scale guessing attacks.

**Slow, tunable hash functions** (bcrypt, PBKDF2, scrypt, Argon2) shift the economic balance by making each guess more expensive.

Hashing should match the *threat model*:

| Hash Type           | Intended Use            | Suitable for Passwords? |
| ------------------- | ----------------------- | ----------------------- |
| MD5, SHA-1, SHA-256 | Integrity, checksums    | No                      |
| bcrypt, PBKDF2      | Password storage        | Yes                     |
| scrypt, Argon2      | Modern password storage |  Recommended            |

---

## How Hashing Relates to Password Security

Hashing affects password security in two key ways:

1. **It determines how expensive a guessing attack is**  
   - Fast hashing → cheap guesses  
   - Slow hashing → expensive guesses  
   - Memory-hard hashing → expensive *and* resource-constrained

2. **It shapes the attack surface in a breach**  
   - Salted hashes slow down broad attacks  
   - Unique salts isolate damage  
   - Iterations reduce throughput at scale

A solid understanding of hashing prevents misinterpretation of risk and strengthens every other part of the security analysis workflow. Moving deliberately through the identification phase results in significant time savings once you begin executing your test plan.

[[1. Concepts|Concepts]]
[[Home]]


#research  #education #fundamentals 