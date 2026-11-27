# Week 3 Password Cracking & Cryptographic Assessment Report (`W3-PM1` & `W3-PM2`)

**Author:** Ramadan Adesoye (`Bolt-123`)  
**Date:** November 27, 2026  
**Program:** Networkwalks Cybersecurity Lab — Week 3  
**Environment:** Windows 11 Host (`John the Ripper 1.9.0-jumbo-1`, `Johnny GUI 2.2`, Google Chrome)  

---

## 1. Executive Summary
During Week 3, offline dictionary-based password recovery was performed against an encrypted target document (`My Locked PDF1.pdf`, 128-bit PDF Revision 4 encryption) across two distinct toolchains:
1. **Browser-Based Analysis (`W3-PM2`):** Extracted the `$pdf$` cryptographic hash via the Networkwalks Hash Calculator and recovered the plaintext password using the Networkwalks Password Cracker.
2. **Local Desktop Cracking (`W3-PM1`):** Loaded the extracted `$pdf$4*4*128*...` hash into Johnny GUI backed by John the Ripper `1.9.0-jumbo-1` to validate offline recovery performance.

---

## 2. Lab Execution & Findings

| Module | Toolchain | Target / Input | Recovered Password | Verification Flag |
| :--- | :--- | :--- | :--- | :--- |
| **`W3-PM2`** | Networkwalks Hash Calculator + Password Cracker | `My Locked PDF1.pdf` (`$pdf$4*4*128*...`) | `password1` | `nw{networkwalks_flag1_jtr_270521_1}` |
| **`W3-PM1`** | John the Ripper `1.9.0-jumbo-1` + Johnny GUI `2.2` | `My-Locked-PDF1.hash` | `password1` | Verified against target PDF |

### Methodology Breakdown
1. **Hash Extraction:** Parsed the PDF encryption dictionary to isolate the `pdf2john` / Hashcat-compatible string (`$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*...`).
2. **Dictionary Attack:** Executed wordlist matching against the extracted hash in both the browser engine and Johnny GUI (`[format=PDF]`), achieving a `100% (1/1)` match on **`password1`**.
3. **Access Validation:** Unlocked `My Locked PDF1.pdf` using `password1` and retrieved Flag 1 (`nw{networkwalks_flag1_jtr_270521_1}`).

---

## 3. Security Observations & Remediation
* **Vulnerability — Weak Common Password:** Despite utilizing 128-bit encryption, protecting the document with a top-100 dictionary word (`password1`) allowed instant offline recovery without brute-force overhead.
* **Recommendation — Enforce High-Entropy Passphrases & Modern Encryption:** Require passwords of at least 14–16 characters combining random words, uppercase/lowercase letters, digits, and symbols, and upgrade document security standards to AES-256 (PDF 2.0 / Revision 6) to increase key-derivation computational cost.