# SHE (Sentinel for Handheld Equipment): Data Privacy & Safety Policy

**Version**: 0.3.0  
**License**: Apache 2.0  
**Compliance Target**: Indian ERSS 112 & Sanchar Saathi CEIR Handset Defense Guidelines

---

## 1. Zero Harm & Safety-First Operating Principle

SHE is designed for anti-theft tracking, device defense, and personal handset recovery with strict safety guarantees:
1. **Zero Destructive Commands**: The system contains **no automated factory wipe**, **no filesystem corruption**, and **no bricking commands**. Personal data and handset operating systems remain physically safe.
2. **Zero Automated Emergency Interference**: The application **does not autonomously dial 112, 911, or any emergency service**. All emergency response references (ERSS 112, state police helplines) are provided purely as user-facing documentation and voluntary informational guides.
3. **Owner-Authorized Control Only**: Every remote command (`LOCATE`, `RING`, `STOP_RING`, `LOCK`) requires cryptographic authorization using HMAC-SHA256 signatures generated on the paired Admin handset using shared secrets established during mutual QR code scanning.

---

## 2. Zero-Telemetry Architecture

Unlike commercial tracking apps that sell device logs and behavioral telemetry to advertising networks, SHE operates on an uncompromising **Zero-Telemetry** standard:
* **0 Advertising SDKs**: No Facebook Graph, Google Ads, Adjust, or AppsFlyer dependencies.
* **0 Cloud Tracking Analytics**: No Firebase Analytics or external user tracking scripts.
* **0 WebViews in Core Workflows**: Completely native Kotlin codebase to prevent cross-site scripting (XSS) and session hijacking vulnerabilities.
* **Minimal Server Footprint**: The backend server is purely an authenticated cryptographic relay. Handset locations are stored with a strict 30-day auto-purge TTL.

---

## 3. Cryptography & Key Management

* **Hardware Keystore Protection**: On-device keys are generated inside Android's Hardware-backed Keystore (Trusted Execution Environment / StrongBox). Private keys cannot be extracted from the device even if ADB is enabled.
* **Zero-Knowledge Vault**: Passwords and sensitive recovery notes stored in the Admin Vault are encrypted client-side using **Argon2id** key derivation (64 MB memory, 3 iterations) and **AES-256-GCM** authenticated encryption. The backend database stores only opaque ciphertexts and cannot decrypt stored secrets.
* **HMAC-SHA256 Command Authentication**: Every command dispatched over SMS or HTTP is authenticated using a shared 256-bit symmetric secret. Replay attacks are prevented via monotonically incrementing sequence numbers and a strict 5-minute time window.

---

## 4. SMS Privacy & Selective Filter Architecture

Because Android grants `RECEIVE_SMS` at the OS level, SHE implements an uncompromising kernel-level inbound message filter:
* Inbound SMS messages are matched **strictly against the `AT1` Protocol Data Unit header**.
* All personal, commercial, and **banking OTP SMS messages are immediately discarded without logging, caching, or disk persistence**.
* Only messages carrying a cryptographically valid 48-bit truncated HMAC signature signed with the paired Admin key are parsed.

---

## 5. Official Indian Law Enforcement & CEIR Compliance

If your device is stolen or lost:
1. Contact the National Emergency Response Support System at **112**.
2. Block the handset's IMEI through the Department of Telecommunications **Sanchar Saathi CEIR Portal** at [sancharsaathi.gov.in](https://sancharsaathi.gov.in).
3. SHE assists legitimate device owners in presenting verified satellite timestamped GPS telemetry to local law enforcement authorities.
