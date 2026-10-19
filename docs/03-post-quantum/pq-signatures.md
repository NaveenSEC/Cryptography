# ✍️ Post-Quantum Signatures

[🏠 Back to index](../../README.md)  ·  **Status legend:** ✅ Recommended  ·  ⚠️ Legacy / use with care  ·  ❌ Broken / do not use

---

## How ML-DSA signs: "Fiat–Shamir with aborts"

```mermaid
flowchart LR
    M["Message"] --> H["Hash message +<br/>random mask y"]
    SK["Secret key s<br/>(small vectors)"] --> Z
    H --> CH["Challenge c"] --> Z["z = y + c·s"]
    Z --> CHK{"Does z leak<br/>info about s?"}
    CHK -- "Yes: reject & retry<br/>with new y" --> H
    CHK -- "No" --> SIG["Signature (z, c)<br/>~2.4–4.6 KB"]
    SIG --> V["Verifier checks with<br/>public key: A·z − c·t ≈ A·y"]
```

The "abort" loop is the clever part: any candidate signature that could statistically reveal the secret key is thrown away, so published signatures are independent of `s`.

## How SLH-DSA works: signatures built only from hashes

```mermaid
flowchart TD
    ROOT["🌳 Public key = root hash<br/>of a huge hypertree"] --> T1["Tree layer"]
    T1 --> T2["Tree layer"]
    T2 --> T3["… (many layers)"]
    T3 --> OTS["One-time / few-time<br/>hash signatures (FORS)"]
    OTS --> MSG["✍️ Signs the message"]
    MSG -.-> PATH["Signature = leaf signature +<br/>authentication path up to the root"]
```

Security relies **only** on the hash function (SHA-2 / SHAKE) being secure, the most conservative assumption available. The cost is large signatures (about 8–50 KB) and slower signing.

## Size comparison: the price of quantum safety

```mermaid
xychart-beta
    title "Public key + signature size in bytes (128-bit security level)"
    x-axis ["Ed25519", "ECDSA P-256", "RSA-3072", "FN-DSA-512", "ML-DSA-44", "SLH-DSA-128s"]
    y-axis "Bytes" 0 --> 8500
    bar [96, 128, 768, 1563, 3732, 7888]
```

| Algorithm | Public key | Signature / ciphertext | Quantum-safe |
|---|---|---|---|
| Ed25519 | 32 B | 64 B signature | ❌ |
| RSA-3072 | 384 B | 384 B signature | ❌ |
| X25519 | 32 B | 32 B share | ❌ |
| ML-KEM-768 | 1,184 B | 1,088 B ciphertext | ✅ |
| FN-DSA-512 | 897 B | ~666 B signature | ✅ |
| ML-DSA-44 | 1,312 B | 2,420 B signature | ✅ |
| SLH-DSA-128s | 32 B | 7,856 B signature | ✅ |

---

⬅️ [🔑 Key Encapsulation (KEM) and ML-KEM](ml-kem.md)  ·  🏠 [Index](../../README.md)  ·  [🔀 Hybrid Post-Quantum Cryptography](hybrid.md) ➡️
