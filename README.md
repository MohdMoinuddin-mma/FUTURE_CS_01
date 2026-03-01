# FUTURE_CS_01 — Vulnerability Assessment Report
### Future Interns Cyber Security Internship · Task 1 · 2026

---

## 📋 Overview

This repository contains the full deliverable for **Task 1** of the Future Interns Cyber Security Internship Programme (Track Code: CS). The task involved performing a professional, ethical, read-only vulnerability assessment of a real public website and documenting findings in a client-ready security report.

> ⚠️ **Educational & Ethical Disclosure Notice**
> All testing was conducted passively under Bose Corporation's authorised security research programme on **HackerOne** (hackerone.com/bose_vdp). No systems were attacked, no data was accessed, and no harm was caused. This report is prepared for educational and internship purposes only.

---

## 🌐 Website Tested

| Field | Detail |
|---|---|
| **Primary Target** | www.bose.com |
| **Additional Assets** | worldwide.bose.com · support.bose.com · www.bose.ae |
| **Organisation** | Bose Corporation |
| **Authorisation** | HackerOne VDP — Bose Gold Standard Safe Harbor |
| **HackerOne Programme** | hackerone.com/bose_vdp |
| **Audit Date** | February 25, 2026 |
| **Auditor** | Mohammad Moinuddin |

---

## 🎯 Scope

### In Scope
- `www.bose.com` — Primary domain
- `*.bose.com` — All subdomains (wildcard)
- Public-facing pages only
- Passive, read-only analysis

### Out of Scope
- Login bypass or authentication testing
- Exploitation of any kind
- Brute force or denial-of-service
- Any non-public or internal systems
- Third-party services not owned by Bose

### Testing Methodology
All testing followed the **OWASP Web Security Testing Guide (WSTG)** using only passive, read-only techniques. No harmful actions were performed at any point.

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| `curl` | HTTP header analysis and response inspection |
| `nmap` | Port scanning and SSL/TLS cipher enumeration |
| `whatweb` | Web technology fingerprinting |
| `dig` | DNS record analysis (SPF, DMARC, DKIM) |
| `base64` | Decoding of leaked response header metadata |
| **Kali Linux 2025** | Testing environment (VMware) |

---

## 📊 Findings Summary

| ID | Finding | Severity | Asset |
|---|---|---|---|
| BOSE-001 | Internal Kubernetes & Cloud Metadata Exposed | 🔴 Critical | worldwide.bose.com |
| BOSE-002 | Missing HSTS on Primary Domain | 🟠 High | www.bose.com |
| BOSE-003 | Wildcard CORS Policy | 🟠 High | worldwide.bose.com |
| BOSE-004 | Session Cookies Missing HttpOnly Flag | 🟠 High | www.bose.com |
| BOSE-005 | Technology Stack Disclosure | 🟡 Medium | www.bose.com, bose.ae |
| BOSE-006 | Incomplete Content Security Policy | 🟡 Medium | www.bose.com |
| BOSE-007 | HSTS max-age Too Short | 🟡 Medium | www.bose.ae |
| BOSE-008 | Missing Security Headers | 🟡 Medium | www.bose.com |
| BOSE-009 | Legacy Hardcoded Expiry Dates | 🟢 Low | www.bose.com |
| BOSE-010 | Example Email in Page Source | 🟢 Low | www.bose.com |

**Overall Risk Rating: HIGH** — 1 Critical · 3 High · 4 Medium · 2 Low

---

## 📁 Repository Structure

```
FUTURE_CS_01/
│
├── README.md                          ← This file
│
├── report/
│   └── Bose_Vulnerability_Assessment_Report_FUTURE_CS_01.pdf
│
└── evidence/
    ├── Screenshot_2026-02-26_002901.png   → BOSE-009 (Legacy expiry dates)
    ├── Screenshot_2026-02-26_002951.png   → Positive control (TLS Grade A)
    ├── Screenshot_2026-02-26_003930.png   → Positive control (SPF + DMARC)
    ├── Screenshot_2026-02-26_004105.png   → BOSE-004 (Cookies) + BOSE-006 (CSP)
    ├── Screenshot_2026-02-26_004208.png   → BOSE-002 (No HSTS) + BOSE-005 (Tech stack)
    ├── Screenshot_2026-02-26_004229.png   → BOSE-001 (Envoy metadata) + BOSE-003 (CORS)
    ├── Screenshot_2026-02-26_005142.png   → Positive control (DKIM)
    ├── Screenshot_2026-02-26_005213.png   → BOSE-001 (base64 decode — K8s metadata)
    └── Screenshot_2026-02-26_005228.png   → BOSE-005 + BOSE-007 (bose.ae headers)
```

---

## 🔑 Key Finding — BOSE-001 (Critical)

The most critical finding was that `worldwide.bose.com` was leaking internal Kubernetes and AWS infrastructure metadata through a response header (`x-envoy-peer-metadata`). Decoding this header revealed:

```
CLUSTER_ID=Kubernetes
INSTANCE_IPS=10.106.72.149
ISTIO_VERSION=1.19.0
AWS_REGION=eu-west-1
AWS_INSTANCE_ID=i-08362642f66f0bddf
NAMESPACE=pulsar-v1
```

This was discovered through a simple `curl` command and base64 decode — no attack required. The fix is a one-line server configuration change to strip the header before public responses.

---

## ✅ Positive Security Controls Found

Despite the vulnerabilities, Bose already has several strong security measures in place:

- **TLS Grade A** — Strong cipher suites confirmed via nmap
- **DMARC `p=reject`** — Strict email spoofing protection
- **SPF record** — Correctly configured
- **DKIM** — Digital email signatures present
- **HTTPS enforcement** — All sites redirect HTTP → HTTPS
- **X-Content-Type-Options: nosniff** — MIME-type sniffing prevented

---

## 📌 Internship Details

| Field | Detail |
|---|---|
| **Programme** | Future Interns Cyber Security Internship 2026 |
| **Track** | Cyber Security (CS) |
| **Task** | Task 1 — Vulnerability Assessment Report for a Live Website |
| **Repository Name** | FUTURE_CS_01 |
| **Intern** | Mohammad Moinuddin |

---

*All findings were identified through passive, read-only methods under the authorised HackerOne VDP scope. No systems were harmed, exploited, or modified in any way.*
