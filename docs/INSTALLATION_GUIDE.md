# SHE Sideloading & Hardware Installation Guide

This guide provides step-by-step instructions for sideloading and configuring the **SHE Guardian Handset** (`she-guardian.apk`) and **SHE Admin Companion** (`she-admin.apk`) on modern physical Android devices (Android 10 through 15).

---

## 1. Prerequisites & APK Verification

Before transferring APKs to your phones, verify the cryptographic integrity of the downloaded files:

```bash
# Verify checksums using sha256sum
sha256sum -c releases/checksums.sha256
```

Expected output:
```text
she-guardian.apk: OK
she-admin.apk: OK
```

---

## 2. Installing on the Guardian Handset (Protected Device)

### Step 2.1: Allow Installation from Unknown Sources
1. Open your device's browser or File Manager where you saved `she-guardian.apk`.
2. Tap the APK file to begin installation.
3. If prompted: *"For your security, your phone is not allowed to install unknown apps from this source"*:
   * Tap **Settings**.
   * Toggle **Allow from this source** to **ON**.
   * Return and tap **Install**.

### Step 2.2: Google Play Protect Advisory
Because SHE requests high-level anti-theft capabilities (Device Administrator, SMS command reception, Direct Boot GPS foreground service) outside the Google Play Store, Google Play Protect may display a warning dialog:
* *Alert*: "Blocked by Play Protect" or "Unrecognized developer".
* *Action*: Tap **"More details"** (small text dropdown) $\rightarrow$ Tap **"Install anyway"**.

> [!NOTE]
> Google Play restricts consumer background SMS and Device Administrator apps. Because SHE is a specialized zero-telemetry defense tool, direct sideloading with Play Protect approval is the intended installation path.

### Step 2.3: Bypass Android 13+ "Restricted Settings" (Crucial)
Starting with Android 13, the operating system disables Accessibility and Device Administrator toggles for sideloaded apps with a greyed-out button: *"Restricted setting: For your security, this setting is currently unavailable"*.

To unlock these permissions:
1. Open phone **Settings** $\rightarrow$ **Apps** $\rightarrow$ **All apps**.
2. Select **SHE Guardian**.
3. In the top-right corner, tap the **Three Dots (⋮)** menu.
4. Tap **"Allow restricted settings"**.
5. Authenticate with your phone's fingerprint or PIN.
6. Return to SHE Guardian $\rightarrow$ tap **Activate Device Administrator**.

### Step 2.4: OEM-Specific Battery & Autostart Settings
To ensure 24/7 background recovery and Direct Boot operation when the phone reboots:
* **Xiaomi / POCO / Redmi (HyperOS / MIUI)**:
  * Open **Settings** $\rightarrow$ **Apps** $\rightarrow$ **Manage apps** $\rightarrow$ **SHE Guardian**.
  * Enable **Autostart**.
  * Set Battery Saver to **"No restrictions"**.
* **Samsung (One UI)**:
  * Open **Settings** $\rightarrow$ **Apps** $\rightarrow$ **SHE Guardian** $\rightarrow$ **Battery**.
  * Change from *Optimized* to **Unrestricted**.
* **OnePlus / OPPO / Realme (OxygenOS / ColorOS)**:
  * Open **Settings** $\rightarrow$ **Battery** $\rightarrow$ **More settings** $\rightarrow$ **Optimize battery use** $\rightarrow$ Set SHE Guardian to **Don't optimize**.

---

## 3. Installing on the Admin Handset (Controller Device)

1. Sideload `she-admin.apk` onto the owner's primary device using the same Unknown Sources procedure.
2. Launch **SHE Admin Companion**.
3. Create your local Master Passphrase for the Zero-Knowledge Vault.
4. Set up TOTP Two-Factor Authentication (scan the 2FA QR code with Google Authenticator, Aegis, or Bitwarden).

---

## 4. Mutual QR Camera Pairing

To link the Guardian phone to the Admin phone with zero cleartext key exchange:
1. On the **Admin Companion**: Tap **"+ Pair New Device"**. The Admin phone displays an encrypted registration QR code containing a one-time cryptographic pairing challenge.
2. On the **Guardian Handset**: Tap **"Scan Admin Pairing QR"**. Scan the QR code displayed on the Admin screen.
3. The handsets negotiate an ECDSA / HMAC-SHA256 session key.
4. Verify the 6-digit confirmation code displayed on both phone screens.
5. Tap **Confirm Pairing**. Both phones are now cryptographically paired!

---

## 5. Live Verification & Testing Commands

Once paired, test the communication channel:
1. From the Admin Companion Dashboard, tap **"Locate"**.
2. The Guardian phone immediately locks on to GPS satellites and transmits an encrypted location fix.
3. On the Admin Companion, view the updated coordinates and tap **Search on Map** to view the live pin in Google Maps.
4. Tap **"Ring"** to test the emergency siren (verify high-pitch alert sounds).
5. Tap **"Stop Ring"** to silence the alert.
