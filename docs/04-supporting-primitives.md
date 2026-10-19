# 🧩 Supporting Primitives: Hashes, MACs, KDFs

[🏠 Back to index](../README.md)  ·  **Status legend:** ✅ Recommended  ·  ⚠️ Legacy / use with care  ·  ❌ Broken / do not use

---

Ciphers alone aren't enough; real systems also need these building blocks.

| Primitive | Purpose | Recommended | Avoid |
|---|---|---|---|
| **Hash function** | Fingerprint data (one-way, collision-resistant) | SHA-256, SHA-384, SHA-3, BLAKE2/BLAKE3 | MD5 ❌, SHA-1 ❌ |
| **MAC** | Prove integrity + authenticity with a shared key | HMAC-SHA256, Poly1305, GMAC | Home-made `hash(key‖msg)` |
| **KDF** | Derive keys from a shared secret | HKDF | Using a raw DH output as a key |
| **Password hashing** | Slow, memory-hard storage of passwords | Argon2id, scrypt, bcrypt | Plain SHA-256, MD5 |

---

⬅️ [🔀 Hybrid Post-Quantum Cryptography](03-post-quantum/hybrid.md)  ·  🏠 [Index](../README.md)  ·  [🌐 TLS 1.2 vs TLS 1.3](05-tls.md) ➡️
