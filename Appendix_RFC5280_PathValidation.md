# Appendix: RFC 5280 Certificate Path Validation  
### (Concise Technical Summary)

RFC 5280 defines the formal algorithm for validating a certificate path  
from a leaf certificate to a trust anchor.  
This appendix provides a concise, engineering-oriented summary of the process,  
aligned with PKI Zoo’s *Path* and *Revocation* models.

---

## 🟩 1. Trust Anchor Selection

A trust anchor is a locally configured root CA certificate.

- Select a trust anchor from the system trust store.
- Validation *must* terminate at this anchor.
- The trust anchor’s self-signature is not validated.

---

## 🟦 2. Basic Certificate Checks (Leaf → Root)

For each certificate in the chain:

- **Signature validation** using parent public key  
- **Validity period** (NotBefore / NotAfter)  
- **Key Usage** (digitalSignature, keyCertSign, etc.)  
- **Extended Key Usage** (serverAuth, clientAuth, etc.)  
- **Critical extensions** MUST be understood and processed  
- **Basic Constraints**  
  - CA:TRUE for intermediates  
  - pathLenConstraint enforcement

If any check fails → path is invalid.

---

## 🟧 3. Name Constraints Processing

Applied from the trust anchor downward:

- **Permitted Subtrees**  
  Allowed DNS names, email domains, IP blocks  
- **Excluded Subtrees**  
  Explicitly forbidden namespace regions  

Leaf’s Subject/SAN must fall within permitted ranges  
and outside excluded ones.

---

## 🟨 4. Certificate Policies

Policy processing controls what “uses” a certificate is valid for.

Major components:

- **Certificate Policies** OIDs  
- **Policy Mappings** between organizations  
- **Policy Constraints**  
  - inhibitPolicyMapping  
  - requireExplicitPolicy  
  - inhibitAnyPolicy  
- **anyPolicy** behavior  
  - Can act as a wildcard unless inhibited

---

## 🟪 5. Path Construction Rules

For each child–parent pair:

- Parent must have issued the child  
- Basic Constraints CA:TRUE (except leaf)  
- Path length constraints enforced  
- Algorithm constraints (signature algorithms / strengths)

This is where many *Path-based failures* occur in PKI Zoo  
(e.g., broken intermediates, cross-sign issues, looped chains).

---

## 🟥 6. Revocation Checking

RFC 5280 permits multiple revocation mechanisms:

### CRL
- Full CRL and Delta CRL  
- Must validate CRL signature and freshness  
- Check revoked serial numbers

### OCSP
- Good / Revoked / Unknown  
- Response signature validation (OCSP responder cert)  
- Nonces (if used)  
- OCSP stapling accepted by browsers/servers

If revocation cannot be checked, implementations diverge:
- Browsers: “soft-fail” acceptable  
- Enterprise PKI: usually “hard-fail”

---

## 🟫 7. Final Path Validation Decision

A certificate path is valid **only if all** of the following pass:

1. Trust anchor reached  
2. All basic checks passed  
3. Name constraints satisfied  
4. Policy processing completed without violation  
5. Chain structure valid (issuer/subject)  
6. Revocation status acceptable  
7. All required extensions understood

Any failure → *path invalid*.

---

## 📌 Notes for PKI Zoo Integration

- Path validation is tightly coupled with **Name** and **Revocation**  
- Most real-world failures derive from:  
  - wrong trust anchor  
  - broken intermediate  
  - expired intermediate  
  - mis-issued cross-sign  
  - AIA not reachable  
  - inconsistent key usage / EKU  
  - soft-fail vs hard-fail policy differences  
- PKI Zoo’s full Path model maps directly to RFC 5280 §6.1.x

---

## 📚 References

- RFC 5280: *Internet X.509 Public Key Infrastructure Certificate and CRL Profile*  
- CAB Forum Baseline Requirements  
- NIST SP 800-52r2 (TLS Guidelines)  
- Microsoft / Mozilla Root Program Documentation

