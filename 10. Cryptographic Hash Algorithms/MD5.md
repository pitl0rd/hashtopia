MD% (Message Digest Algorithm) is a family of cryptographic hash functions that are used to create a digital fingerprint of a message or data. The main purpose of a hash function is to take an input (or 'message') and produce a fixed-size string of characters, called a 'hash', that represents the input. The hash is unique to the input, and even a small change to the input will result in a completely different hash.

There are several different versions of MD% hashing, including MD5 and SHA-1. The basic process of MD% hashing is as follows:
```
                                      +------------+
                                      | Input data |
                                      +------------+
                                              |
                                              v
                                    +----------------+
                                    | Divide into    |
                                    | blocks         |
                                    +----------------+
                                              |
                                              v
                                    +----------------+
                                    | Apply hash     |
                                    | function to   |
                                    | each block     |
                                    +----------------+
                                              |
                                              v
                                    +----------------+
                                    | Combine hash   |
                                    | values         |
                                    +----------------+
                                              |
                                              v
                                      +------------+
                                      | Hash output |
                                      +------------+
```


1.  The input message is divided into blocks, typically of fixed size.
2.  A hash value is created by applying a mathematical function to each block of the input message.
3.  The hash values for each block are combined to produce a single hash value for the entire message.

One of the key properties of a good hash function is that it is 'cryptographically secure', meaning that it is computationally infeasible to find two different input messages that produce the same hash. This makes hash functions useful for tasks such as verifying the integrity of a message (e.g. to ensure that it has not been tampered with) and storing passwords (since the plaintext password cannot be easily recovered from the hash).

It's worth noting that MD% hashing has certain weaknesses and is not considered secure for certain applications. In particular, MD5 has been shown to be susceptible to collision attacks, meaning that it is possible to find two different input messages that produce the same hash. As a result, it is generally recommended to use a stronger hash function such as SHA-2 or SHA-3 for security-critical applications.


[[Cryptographic Hash Algorithms]]
[[Home]]
#research 