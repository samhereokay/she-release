# Release Notes & Distribution Ledger

Official signed binary builds, cryptographic verification hashes, and changelog for the **SHE (Sentinel for Handheld Equipment)** ecosystem.

---

## Current Release: v0.3.0 (Hardware Verification & Telemetry Edition)

**Release Date**: October 2026  
**Status**: Stable Community Pre-Release  
**License**: Apache 2.0 Open-Source License  

### Included Binary Artifacts
| Application | Binary File | Size | Architecture | SHA-256 Checksum |
| :--- | :--- | :--- | :--- | :--- |
| **SHE Guardian** | [`she-guardian.apk`](releases/she-guardian.apk) | ~33.7 MB | ARM64-v8a / x86_64 | `f43a804f84fa92d54a6d7a07eb625dac9c8627ee7670abd3c67b3fa17e03b037` |
| **SHE Admin Companion** | [`she-admin.apk`](releases/she-admin.apk) | ~31.0 MB | ARM64-v8a / x86_64 | `3fafa29182a74b648109d2f225ab652e5c677042d9fa5f063ef3fc42a131b59a` |

### Key Features & Capabilities in v0.3.0
1. **🛰️ WebSocket Map View Live Tracking (Admin APK)**:
   * Dedicated real-time radar tracking activity inside `she-admin.apk`.
   * Live streaming WebSocket connection to `/api/v1/ws/devices/{device_id}` with automated keepalive ping and seamless auto-reconnect.
   * Interactive Leaflet dark tactical map with CartoDB Dark Matter tiles, glowing cyan breadcrumb polyline, pulsing radar animation, accuracy range ring, and auto-pan follow target.
   * On-map tactical actions: instant `LOCATE` ping dispatch, siren alert trigger, and interactive demo simulation mode.
2. **🗺️ Interactive Map Search & Google Maps Fallback**:
   * One-click **"Maps"** button on device cards to view fixes in external navigation apps (`geo:<lat>,<lng>?q=...`).
   * Satellite accuracy indicators ($\pm \text{meters}$) and capture vs. server receipt timestamps.
3. **ℹ️ In-App Architecture & Telemetry Modal**:
   * Detailed explanations of Direct Boot hardware storage, Device Encrypted Storage (DE), and zero-telemetry backend assurances accessible directly from the app interface.
3. **Mutual Camera QR Pairing**:
   * Out-of-band cryptographic handshake between handsets without exposing secrets over Wi-Fi or Bluetooth.
   * Six-digit visual verification code on both screens to defeat Man-in-the-Middle (MITM) attacks.
4. **Offline Emergency AT1 SMS Beacons**:
   * Autonomous SMS fallback when mobile data and Wi-Fi are disconnected.
   * 48-bit truncated HMAC-SHA256 signatures with replay defense.
5. **Zero-Knowledge Vault**:
   * Client-side Argon2id key derivation with AES-256-GCM encryption for stored passphrases and device emergency recovery tokens.
6. **Strict Safety & Non-Destructive Principles**:
   * **Zero Device Damage**: No destructive filesystem corruptions, factory wipes, or device bricking.
   * **Zero Automated Emergency Dialing**: No autonomous calls to 112 or police dispatchers. Emergency references are purely informational.

### Verifying Hashes
To guarantee the APKs have not been tampered with or modified:
```bash
# Verify integrity using the included checksum file
cd releases
sha256sum -c checksums.sha256
```

Expected result:
```text
she-guardian.apk: OK
she-admin.apk: OK
```
