# vessel-edn-dos-research

*If this research helped you, please consider giving it a ⭐ Star.*


## 🚀 Stay Updated
Found this research useful?
* **Star ⭐** this repo to keep track of it.
* **Follow me** on GitHub for more DeFi security research.
* **Fork** it if you want to run your own experiments.

### ☕ Support the Research
If you appreciate the work and want to support further security research:

<img src="456.PNG" alt="Donate QR" width="200"/>

**Wallet Address (ETH/EVM):** 0xBDDD7973D0DE27B715A4A5cbdb87d0DF78757b3A 


# Research: Application-Level DoS via Unsafe EDN Parsing

## Overview
This research explores a Denial of Service (DoS) vulnerability in the [Vessel](https://github.com/nubank/vessel) tool. The issue stems from the use of unsafe EDN (Extensible Data Notation) parsing logic, specifically via the `vessel.misc/read-string` function.

## Technical Deep Dive
In Clojure, using the standard `clojure.core/read-string` on untrusted input is dangerous because it can trigger the evaluation of arbitrary reader macros or cause extreme memory consumption during the parsing of deeply nested structures.


### Vulnerable Code Pattern
The vulnerability was identified in `src/vessel/misc.clj`, where input from external sources is processed without a secure parser like `clojure.edn/read-string`.

## Proof of Concept
A specially crafted EDN payload (e.g., using recursive data structures or massive strings) can be passed to the tool, causing the JVM to hang or crash with an `OutOfMemoryError`.

```clojure
;; Example of a dangerous EDN structure that can cause parsing overhead
#inst "9999-12-31T23:59:59.999-99:99" 
;; Or deeply nested vectors: [[[[[[...]]]]]]
