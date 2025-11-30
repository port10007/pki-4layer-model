# 🌐 PKI 4-Layer Integrated Model (mnr.dev Edition)

This repository contains a unified four-layer abstraction of Public Key Infrastructure (PKI),  
combining **cryptographic purpose, TLS validation logic, CA governance, and certificate lifecycle operations**.  
The model originates from the *PKI Zoo* research project.

📘 Available documents:
- **PKI_4Layer_Model_JP.md** — 日本語版  
- **PKI_4Layer_Model_EN.md** — English Edition  
- **Appendix_RFC5280_PathValidation.md** — RFC 5280 Path Validation Summary (EN)

---

## 📚 Overview

Understanding PKI requires four separate but interconnected perspectives:

### 🟦 Layer 1: Cryptographic Foundation  
Authentication, Integrity, Confidentiality — the security goals PKI enables.

### 🟩 Layer 2: TLS Validation Structure (RFC 5280)  
Name / Path / Revocation — the three pillars of certificate verification.

### 🟧 Layer 3: CA Governance  
Identity verification, authorization, policy control, and auditability.

### 🟨 Layer 4: Certificate Lifecycle  
Registration, issuance, renewal, rollover, revocation, and validation.

Each layer represents a different level of abstraction used in real PKI implementations,  
e-government PKI, corporate CA operations, and TLS ecosystems.

---

## 📌 Appendix: RFC 5280 Path Validation

See: **Appendix_RFC5280_PathValidation.md**

Includes:
- Trust anchor selection  
- Basic certificate checks  
- Name constraints  
- Certificate policy processing  
- Path construction rules  
- CRL / OCSP revocation logic  

This appendix aligns with PKI Zoo's *Path* failure cases.

---

## 🔗 Related Projects

- **PKI Zoo (Name / Path / Revocation modeling)**
- **QUIC Zoo (Handshake / Transport / Path modeling)**
- **Curl Zoo (HTTP/TLS error-space modeling)**  
*(Public release planned on mnr.dev)*

---

## 📄 License

Text documents in this repository are distributed under:

**CC BY 4.0 — Creative Commons Attribution 4.0 International**  
You are free to share and adapt the material with attribution.

Full text: https://creativecommons.org/licenses/by/4.0/

---

## 👤 Author

**Minoru Tachibana (port10007)**  
Researching PKI, TLS/QUIC protocol behavior, certificate ecosystems,  
and low-level system toolchains.

ORCID: *(optional — add later if公開したくなったら)*  
GitHub: https://github.com/port10007

---

## 📬 Contact / Website

mnr.dev（準備中）  
Technical articles, PKI/QUIC Zoo documentation, Hugo-based site.

