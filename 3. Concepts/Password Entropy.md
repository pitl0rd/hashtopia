
Password entropy is a measure of the **theoretical unpredictability** of a password, based on the size of the space from which it is assumed to be drawn. In general, a password with higher entropy is expected to be more difficult to guess or crack than a password with lower entropy _under idealized assumptions_.

There are several factors that contribute to the theoretical entropy of a password:

1. **Length**  
    A longer password generally has higher entropy than a shorter password, because it expands the total number of possible combinations.
    
2. **Character set**  
    A password that uses a larger character set (for example, a mix of letters, numbers, and special characters) will generally have higher theoretical entropy than a password that uses a smaller character set (such as lowercase letters only).
    
3. **Randomness of selection**  
    A password that is randomly generated will generally have higher entropy than a password chosen by a person, because human-chosen passwords tend to contain patterns, reuse, or meaningful components.
    

To estimate entropy, password strength or entropy calculators are often used. These tools typically assume that each character is selected independently and uniformly from the chosen character set. While this assumption rarely holds true for human-generated passwords, it provides a useful **upper bound** for randomness.

In general, higher-entropy passwords provide better resistance against brute-force attacks, particularly when combined with slow or memory-hard hashing algorithms. However, the amount of entropy required depends heavily on the hash type, attack model, and system context. Claims such as “128 bits of entropy” are meaningful only for _uniformly random secrets_ and are not realistic for memorized human passwords.

Password entropy is measured in bits and can be approximated using the following formula:

```
C = size of the character set
L = length of the password

Entropy ≈ log2(C) × L
```

To estimate time-to-crack, benchmarking can be performed using password cracking software against a specific hash mode to determine guesses per second. The table below illustrates estimated brute-force times using an **MD4-class fast hash** on an **8× NVIDIA GTX 1080 GPU system**, assuming **randomly generated passwords**:

|Length|Character Set|Estimated Entropy|Time to Crack|
|---|---|---|---|
|8|a–z, A–Z|~47 bits|~15 minutes|
|9|a–z, A–Z|~54 bits|~14 hours|
|10|a–z, A–Z|~59 bits|~457 hours|
|11|a–z, A–Z|~65 bits|~3.3 years|
|12|a–z, A–Z|~71 bits|~214 years|
|13|a–z, A–Z|~77 bits|~13,690 years|
|14|a–z, A–Z|~80 bits|~109,500 years|
|15|a–z, A–Z|~89 bits|~56,080,000 years|
|20|a–z, A–Z|~119 bits|Practically infeasible|

* These estimates apply **only** to uniformly random passwords and **do not reflect real-world human password selection**.

---

## Entropy vs Guessability 

While entropy describes the _size of the theoretical search space_, **guessability** describes how quickly a password is likely to fall in real attacks based on human behavior and attack ordering.

|Password|Theoretical Entropy|Guessability in Practice|Likely Outcome|
|---|---|---|---|
|`P@ssw0rd!`|High (complex charset)|Very high|Cracked almost immediately|
|`Summer2024!`|Moderate|Very high|Cracked early|
|`CorrectHorseBatteryStaple`|Moderate–High|Medium|Resists longer|
|`orbit-lamp-canvas-river`|High (due to length)|Low|Resists significantly longer|
|`X7$QmP9!`|Very high|Low|Strong if truly random|

**Key distinction:**

- **Entropy** assumes randomness
    
- **Guessability** reflects reality
    

Security outcomes are driven far more by **guessability** than by entropy estimates alone. You can learn more about [[Entropy and Guessability]] here.

---

### Why This Matters

Entropy is a useful theoretical concept, but it **overestimates security** for human-generated passwords. Real-world cracking success depends on **structure, reuse, transformation patterns, and probability concentration**, which entropy calculations do not capture. For this reason, Hashtopia treats entropy as an _upper bound_ and prioritizes **empirical guessability analysis** when assessing real password risk.

[[1. Concepts|Concepts]]
[[Home]]

#Resources #Research #education 

