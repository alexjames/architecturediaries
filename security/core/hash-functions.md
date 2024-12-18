---
layout: page
title: Hash Functions
categories: security
---

### Hashing
A hash function is a one-way mathematical function that converts a string of bytes into a fixed size representation, known as its hash value.

Modern hash functions optimized for securing passwords are deliberaly designed to be slow and memory-hard (consume a lot of memory) to make them harder to brute-force. A memory-hard function limits the effectiveness of specialized cracking hardware such as ASICs or GPUs.

#### Properties of a good hash function
 * **determinism**: the same input always produces the same output
 * **avalance effect**: minor differences in input produces singificant differences in the hash value
 * **pre-image resistance**: it's computationally infeasible for reproduce the original input from the output
 * **collision resistance**: it's hard to find two values that produce the same hash value

#### Main applications
 * check data integrity and checksums
 * store passwords securely
 * data structures like hash tables
 * digital signatures and certificates
 * file deduplication
 * data partitioning

#### Comparison
| Algorithm | Size (bits) | Features  | Applications  |
|---|---|---|---|
| MD5 | 128 | <ul><li>fast to compute</li><li>vulnerable to collision attacks and rainbow tables</li></ul> | non-security critical applications, legacy systems |
| SHA 1 | 160 | stronger than MD5 | collisions possible, deprecated in favor of SHA-2 and SHA-3 |
| SHA 2 | 224/256/384/512 | <ul><li>considered secure</li><li>no practical attack methods yet</li></ul> | widely used in SSL/TLS certs and digital signatures |
| SHA 3 | 224/256/384/512 | <ul><li>different cryptographic approach from SHA-2</li><li>slower than SHA-2</li><li>quantum resistant, potentially more secure than SHA-2</li></ul> | emerging applications with specific security requirements |
| PBKDF2 (Password-Based Key Derivation Function) | 1 bits | <ul><li>uses a salt and multiple iterations</li><li>protects against brute force and rainbow tables</li></li><li>less memory-hard than bcrypt and Argon2</li></ul> | password protection in older systems with limited memory |
| bcrypt | 1 bits | <ul><li>adaptive hash function with built in salt</li><li>adjusts computational cost as hardware improves to deliberately slow down computation</li><li>less memory-hard than Argon2</li></ul> | password protection in general web applications |
| Argon2 | 1 bits | <ul><li>modern highly secure hash function</li><li>configurable memory, time and parallelism cost</li></ul> | high security requirements |