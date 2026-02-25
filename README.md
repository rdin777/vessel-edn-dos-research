# vessel-edn-dos-research

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
