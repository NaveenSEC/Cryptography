# Elliptic Curve Cryptography (ECC)

[🏠 Back to index](../../README.md)  ·  **Status legend:** ✅ Recommended  ·  ⚠️ Legacy / use with care  ·  ❌ Broken / do not use

---

Same ideas as DH and DSA, but performed on points of an elliptic curve `y² = x³ + ax + b` over a finite field.

- **Point addition:** draw a line through two points; where it meets the curve again, reflect across the x-axis.
- **Scalar multiplication:** `Q = k × G` means adding the base point G to itself k times (done quickly with double-and-add).
- **Hard problem (ECDLP):** given `G` and `Q`, find `k`. The best attacks are much weaker relative to key size than for factoring, so **keys can be far smaller**.

| Security level | RSA / DH key | ECC key |
|---|---|---|
| 112-bit | 2048 | 224 |
| 128-bit | 3072 | 256 |
| 192-bit | 7680 | 384 |
| 256-bit | 15360 | 512 |

*(per NIST SP 800-57)*

**ECC variants:**

- **ECDH:** Diffie-Hellman on a curve. Each side computes `(own secret) × (other's public point)`. **X25519** (on Curve25519) is the modern favourite: fast, constant-time, hard to misuse.
- **ECDSA:** the elliptic-curve Digital Signature Algorithm. Each signature needs a random nonce `k`; **reusing or leaking `k` reveals the private key** (this is how the PlayStation 3 signing key was extracted in 2010).
- ➕ **EdDSA / Ed25519:** a modern signature scheme on Edwards curves with **deterministic nonces**, eliminating the ECDSA nonce failure. Fast, compact (64-byte signatures, 32-byte keys). Used by SSH, Signal, Tor, and many others.

---

⬅️ [Diffie-Hellman (DH, 1976)](diffie-hellman.md)  ·  🏠 [Index](../../README.md)  ·  [ElGamal and DSA](elgamal-and-dsa.md) ➡️
