# 🔑 24-Hour TOTP Panel

A fully client-side web tool (no server required) that generates and validates security codes from a secret passphrase. Unlike standard TOTP (which changes every 30 seconds), codes here change **once a day**, making them useful when you need a memorable code that stays stable throughout the entire day.

## Features

- **Two code modes**:
  - **TOTP**: 6-digit numeric code, displayed in 2 blocks of 3.
  - **Pass**: 12-character alphanumeric password (uppercase, lowercase and digits), displayed in 3 blocks of 4.
- **Daily generator**: produces the code for any selected date.
- **Validator**: checks whether an entered code matches the current key and date.
- **Two copy buttons**:
  - 🟠 Copies the code **with spaces** between blocks (more readable).
  - 🟣 Copies the code **without spaces** (ready to paste directly).
- **Key history**: stores up to 5 recently used secret keys, accessible from a searchable dropdown (Select2).
- **Annual export**: generates a full table of codes for any selected year, with paginated preview, search, and export to **CSV**, **Excel**, and **PDF**.
- Runs entirely in the browser — no secret key is ever sent to an external server.

## How it works

1. The **secret key** you enter (any ASCII word or phrase) is processed with SHA-256 to produce a fixed-length value.
2. **TOTP mode**: the hash is Base32-encoded and a 6-digit code is generated using the day number as a counter (a daily-counter HOTP variant).
3. **Pass mode**: the hash is combined with the day number to deterministically derive a 12-character alphanumeric password. The same key and date always produce the same result.

> **Technical note:** TOTP mode is not standard TOTP (RFC 6238) — it is an HOTP variant with a daily counter. It is not compatible with authenticator apps such as Google Authenticator.

## ⚠️ Security disclaimer

**This project is an experimental/educational tool and should NOT be used to protect anything sensitive** (accounts, production systems, personal data, financial access, critical infrastructure, etc.). It has not been designed or audited as a production-grade authentication mechanism.

The code is provided **"as-is", without warranty of any kind**, express or implied. The author is not responsible for loss of access, security breaches, data leaks, direct or indirect damages, or any other consequence arising from the use, misuse, or failure of this code. Use it at your own risk.

## Requirements

- A modern web browser with an internet connection (the panel loads Bootstrap, DataTables, Select2, and otplib from a CDN).
- No installation, backend, or additional dependencies required.

## How to use

### Navigation bar

All global controls are in the top bar:

- **Secret Key**: dropdown with a history of up to 5 keys. Type a new one directly to create it.
- **Date**: date picker used by both the generator and the validator.
- **Method**: choose between `TOTP (6 digits)` and `Pass (12 alphanumeric)`.
- **Today / Date** and **Full Year**: navigation between the two pages. The 🔑 logo also navigates back to the main page.

### 1. Generate a code

1. Open `index.html` in your browser.
2. In the top bar, enter your **Secret Key**, select the **Date** and **Method**.
3. The code will appear automatically in the **Generator** panel.
4. Use the 🟠 button to copy the code with spaces, or the 🟣 button to copy it without spaces.

### 2. Validate a code

1. In the **Validator** panel, make sure the same key and date are set in the top bar.
2. Type the code to verify in the input field.
3. The system will indicate in real time whether the code is **valid** or **incorrect**.

> The secret key and date are automatically synced between the generator and the validator.

### 3. Generate codes for a full year

1. Go to the **Full Year** tab.
2. Select the desired year (2020–2100).
3. Click **Generate CSV** to see the table with all codes for that year.
4. Export the result using the **CSV**, **Excel**, or **PDF** buttons.

## Security considerations

- The secret key is stored in the browser's `localStorage` **in plain text**. Do not use it on shared or public devices.
- Key derivation uses a single SHA-256 (no cost factor such as PBKDF2 or Argon2), so short or common passphrases may be vulnerable to brute-force or dictionary attacks.
- This tool is intended **only** for personal, recreational, or very low-sensitivity use. **Never** as a replacement for a real two-factor authentication system or to protect anything that truly matters.

## Project structure

```
.
├── index.html   # Complete application (HTML + CSS + JS)
└── README.md    # This file
```
