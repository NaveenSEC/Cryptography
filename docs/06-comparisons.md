# 📊 Comparisons and Recommendations

[🏠 Back to index](../README.md)  ·  **Status legend:** ✅ Recommended  ·  ⚠️ Legacy / use with care  ·  ❌ Broken / do not use

---

## Symmetric algorithms

| Algorithm | Type | Block | Key size | Structure | Security today | Speed |
|---|---|---|---|---|---|---|
| **AES** | Block | 128 | 128/192/256 | SPN | ✅ Excellent | ⚡⚡⚡ (with AES-NI) |
| DES | Block | 64 | 56 | Feistel | ❌ Brute-forceable | ⚡⚡ |
| 3DES | Block | 64 | 112 effective | Feistel ×3 | ⚠️ Deprecated | ⚡ |
| Blowfish | Block | 64 | 32–448 | Feistel | ⚠️ 64-bit block | ⚡⚡ |
| Twofish | Block | 128 | 128/192/256 | Feistel | ✅ Strong | ⚡⚡ |
| Serpent | Block | 128 | 128/192/256 | SPN | ✅ Very strong | ⚡ |
| Camellia | Block | 128 | 128/192/256 | Feistel | ✅ Strong | ⚡⚡ |
| IDEA | Block | 64 | 128 | Mixed algebra | ⚠️ Legacy | ⚡⚡ |
| RC4 | Stream | — | 40–2048 | Byte shuffle | ❌ Broken | ⚡⚡⚡ |
| Salsa20 | Stream | — | 256 | ARX | ✅ Strong | ⚡⚡⚡ |
| **ChaCha20** | Stream | — | 256 | ARX | ✅ Excellent | ⚡⚡⚡ (any CPU) |

## Asymmetric algorithms

| Algorithm | Use | Hard problem | Key size (128-bit security) | Quantum-safe | Status |
|---|---|---|---|---|---|
| RSA | Encryption + signatures | Factoring | 3072 | ❌ | ✅ with OAEP/PSS, ≥2048 bits |
| DH | Key exchange | Discrete log | 3072 | ❌ | ✅ ephemeral only |
| ECDH / X25519 | Key exchange | EC discrete log | 256 | ❌ | ✅ Preferred classical |
| ECDSA | Signatures | EC discrete log | 256 | ❌ | ✅ with good nonces |
| Ed25519 | Signatures | EC discrete log | 256 | ❌ | ✅ Preferred classical |
| DSA | Signatures | Discrete log | 3072 | ❌ | ⚠️ No new signatures |
| ElGamal | Encryption | Discrete log | 3072 | ❌ | ⚠️ Legacy |
| ML-KEM | Key encapsulation | Module-LWE | ~1184-byte public key (ML-KEM-768) | ✅ | ✅ Standardized 2024 |
| ML-DSA | Signatures | Module-LWE/SIS | ~1952-byte public key (ML-DSA-65) | ✅ | ✅ Standardized 2024 |
| SLH-DSA | Signatures | Hash security | 32–64-byte public key, large signatures | ✅ | ✅ Standardized 2024 |

## Why the strong ones are strong

| Algorithm | Source of robustness |
|---|---|
| **AES** | Full diffusion in 2 rounds; algebraic S-box resistant to linear/differential attacks; 20+ years of analysis without a practical break |
| **ChaCha20** | Simple ARX design, constant-time by construction, large security margin (best attacks reach ~7 of 20 rounds) |
| **GCM / Poly1305** | Authenticated encryption: any tampering is detected before decryption output is used |
| **RSA** | No efficient classical factoring algorithm for large semiprimes |
| **ECC** | ECDLP has no known sub-exponential classical attack, giving tiny keys for high security |
| **Ed25519 / X25519** | Curves designed to avoid implementation pitfalls (no weak points, deterministic nonces, constant-time) |
| **ML-KEM / ML-DSA** | Lattice problems resist all known classical and quantum algorithms |
| **TLS 1.3** | Removes every legacy option that past attacks exploited; mandatory forward secrecy |

---

## ✅ What Should I Use Today?

| Need | Use | Avoid |
|---|---|---|
| Encrypt data | **AES-256-GCM** or **ChaCha20-Poly1305** | ECB, unauthenticated CBC/CTR, DES, 3DES, RC4 |
| Key exchange | **X25519** (ideally hybrid with **ML-KEM-768**) | Static RSA key exchange, small DH groups |
| Digital signatures | **Ed25519**, ECDSA P-256, RSA-PSS ≥3072, **ML-DSA** for PQ | DSA, PKCS#1 v1.5 for new designs |
| Hashing | **SHA-256 / SHA-3 / BLAKE3** | MD5, SHA-1 |
| Passwords | **Argon2id** (or scrypt / bcrypt) | Any fast hash |
| Transport | **TLS 1.3** | SSL, TLS 1.0, TLS 1.1 |
| Disk encryption | **AES-XTS** | ECB |

> ⚠️ **Don't roll your own crypto.** Use well-audited libraries (libsodium, OpenSSL, BoringSSL, Go's `crypto`, Bouncy Castle, Tink) and their high-level APIs.

---

⬅️ [🌐 TLS 1.2 vs TLS 1.3](05-tls.md)  ·  🏠 [Index](../README.md)
