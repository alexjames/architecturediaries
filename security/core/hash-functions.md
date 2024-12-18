---
layout: page
title: Hash Functions
categories: security
---

### Hashing
A hash function is a one-way mathematical function that converts a string of bytes into a fixed size representation, known as its hash value.

#### Properties of a good hash function
 * **determinism**: the same input always produces the same output
 * **avalance effect**: minor differences in input produces singificant differences in the hash value
 * **pre-image resistance**: it's computationally infeasible for reproduce the original input from the output
 * **collision resistance**: it's hard to find two values that produce the same hash value
