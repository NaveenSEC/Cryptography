# ElGamal and DSA

[🏠 Back to index](../../README.md)  ·  **Status legend:** ✅ Recommended  ·  ⚠️ Legacy / use with care  ·  ❌ Broken / do not use

---

## ⚠️ ElGamal (1985)

Encryption based on the discrete log problem, essentially DH turned into encryption.

- Public key `h = gˣ`, private key `x`.
- **Encrypt** `m`: pick random `k`, output `(c₁, c₂) = (gᵏ, m · hᵏ)`.
- **Decrypt:** `m = c₂ / c₁ˣ`.

It is **randomized** (the same message encrypts differently each time) but ciphertexts are twice the size of the message. Used in older GnuPG keys; now mostly of academic and historical interest. Its signature form inspired DSA.

## ➕ Added: DSA (Digital Signature Algorithm) ⚠️

The US federal signature standard from 1991, based on discrete logs. Like ECDSA, it is fragile with respect to nonce reuse. **FIPS 186-5 (2023) no longer approves DSA for generating new signatures**; use ECDSA or EdDSA instead.

---

⬅️ [Elliptic Curve Cryptography (ECC)](ecc.md)  ·  🏠 [Index](../../README.md)  ·  [⚛️ Post-Quantum Cryptography: Overview](../03-post-quantum/overview.md) ➡️
