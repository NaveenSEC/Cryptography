# ⚛️ Post-Quantum Cryptography: Overview

[🏠 Back to index](../../README.md)  ·  **Status legend:** ✅ Recommended  ·  ⚠️ Legacy / use with care  ·  ❌ Broken / do not use

---

A large quantum computer running **Shor's algorithm** would break RSA, DH, DSA and ECC. **Grover's algorithm** only halves symmetric security (AES-128 → about 64-bit, AES-256 → about 128-bit), so **AES-256 stays safe**.

## What a quantum computer breaks

```mermaid
flowchart LR
    Q["⚛️ Large quantum computer"] --> SH["Shor's algorithm<br/>(exponential speed-up)"]
    Q --> GR["Grover's algorithm<br/>(square-root speed-up)"]

    SH --> X1["RSA ❌"]
    SH --> X2["DH / DSA / ElGamal ❌"]
    SH --> X3["ECDH / ECDSA / Ed25519 ❌"]

    GR --> Y1["AES-128 ⚠️<br/>~64-bit security"]
    GR --> Y2["AES-256 ✅<br/>~128-bit security"]
    GR --> Y3["SHA-256 / SHA-3 ✅<br/>still fine"]

    X1 --> FIX["Replace with<br/>ML-KEM · ML-DSA · SLH-DSA"]
    X2 --> FIX
    X3 --> FIX
    Y1 --> FIX2["Use 256-bit keys"]
```

The urgent threat is **"harvest now, decrypt later"**: adversaries recording encrypted traffic today to decrypt it once quantum computers exist.

```mermaid
sequenceDiagram
    participant A as Alice
    participant E as 🕵️ Attacker
    participant B as Bob
    Note over A,B: Today: classical key exchange (e.g. ECDHE)
    A->>B: 🔒 Encrypted traffic
    A-->>E: Attacker silently records everything
    Note over E: Stores ciphertext for years
    Note over E: Future: quantum computer arrives
    E->>E: Shor's algorithm recovers the ECDHE key
    E->>E: 🔓 Decrypts the recorded traffic
    Note over A,B: Defence: switch to post-quantum key exchange NOW,<br/>even before quantum computers exist
```

## The post-quantum family tree

```mermaid
flowchart TD
    PQ["⚛️ Post-quantum cryptography"] --> L["Lattice-based"]
    PQ --> HB["Hash-based"]
    PQ --> CB["Code-based"]

    L --> MLK["ML-KEM (Kyber)<br/>🔑 Key encapsulation<br/>FIPS 203"]
    L --> MLD["ML-DSA (Dilithium)<br/>✍️ Signatures<br/>FIPS 204"]
    L --> FN["FN-DSA (FALCON)<br/>✍️ Compact signatures<br/>FIPS 206 (in progress)"]
    HB --> SLH["SLH-DSA (SPHINCS+)<br/>✍️ Conservative signatures<br/>FIPS 205"]
    CB --> HQC["HQC<br/>🔑 Backup KEM<br/>selected 2025"]
```

NIST finalized the first post-quantum standards in August 2024:

| Standard | Based on | Purpose | Math family | Notes |
|---|---|---|---|---|
| **ML-KEM** (FIPS 203) ✅ | CRYSTALS-Kyber | Key encapsulation | Module lattices (MLWE) | Fast, ~1 KB keys. Already deployed in browsers |
| **ML-DSA** (FIPS 204) ✅ | CRYSTALS-Dilithium | Signatures | Module lattices | General-purpose default signature |
| **SLH-DSA** (FIPS 205) ✅ | SPHINCS+ | Signatures | Hash-based | Conservative (relies only on hash security), large/slow signatures |
| **FN-DSA** (FIPS 206, in progress) | FALCON | Signatures | NTRU lattices | Smallest PQ signatures, tricky to implement |
| **HQC** (selected 2025) | HQC | Key encapsulation | Error-correcting codes | Backup KEM with different math than ML-KEM |

## How lattice crypto works: Learning With Errors (LWE)

Given a public matrix `A` and `b = A·s + e`, where `e` is a small random "error", recovering the secret `s` is the **Learning With Errors** problem.

```mermaid
flowchart LR
    subgraph EASY["Without noise: EASY"]
        direction TB
        e1["b = A · s"] --> e2["Gaussian elimination"] --> e3["s recovered ✅<br/>(school algebra)"]
    end
    subgraph HARD["With noise: HARD"]
        direction TB
        h1["b = A · s + e<br/>(e = small random errors)"] --> h2["Errors compound through<br/>every elimination step"] --> h3["Search space in hundreds<br/>of dimensions ❌<br/>(no fast classical or<br/>quantum algorithm known)"]
    end
    EASY ~~~ HARD
```

The legitimate receiver knows `s` (the trapdoor), so the small noise can simply be rounded away. An attacker without `s` faces a needle-in-a-haystack problem. ML-KEM uses the *module* variant (MLWE), which works on small polynomial matrices to keep keys compact and fast.

---

⬅️ [ElGamal and DSA](../02-asymmetric/elgamal-and-dsa.md)  ·  🏠 [Index](../../README.md)  ·  [🔑 Key Encapsulation (KEM) and ML-KEM](ml-kem.md) ➡️
