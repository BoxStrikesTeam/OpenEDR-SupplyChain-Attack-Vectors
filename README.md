# OpenEDR-SupplyChain-Attack-Vectors
OpenEDR-SupplyChain-Attack-Vectors



# Open EDR – Supply Chain Vulnerability Disclosure

**Researcher:** Eneshan Erdoğan Karaca  
**GitHub:** [Kisaca-Enes](https://github.com/Kisaca-Enes)  
**Report Date:** July 10, 2026  
**Status:** Full Disclosure

---

## 📌 Summary

Open EDR (Xcitium / Comodo) bundles **severely outdated** third-party libraries that have not been updated for **7–8 years**. These libraries contain **12 directly identifiable CVEs** and **over 100 indirect CVEs**, including critical **Remote Code Execution (RCE)**, **Information Disclosure**, **Denial of Service (DoS)**, and **Security Bypass** vulnerabilities.

The vendor was notified on **June 21, 2026** and acknowledged the issue on **June 23**, stating they would update. However, **4 follow-up emails** were ignored, and no further communication was received. This report is published after a **responsible disclosure** period to protect users and raise awareness.

---

## 🔍 What is a Supply Chain Attack?

A supply chain attack targets a software product through its **external dependencies**, rather than its own code. If an application uses a vulnerable library, that vulnerability propagates to the application itself.

Open EDR relies on libraries such as **cURL**, **OpenSSL**, **Boost**, and **AWS SDK**. Any known vulnerability in these libraries directly affects Open EDR, making it an attractive target for attackers.

> **Example:** A company uses vendor X's software. Vendor X uses an outdated OpenSSL version. An attacker exploits that OpenSSL flaw to compromise vendor X's software, then pivots into the company's network.

---

## 📦 Affected Components & Vulnerabilities

| ID | Library | Version | CVE | Impact |
|----|---------|---------|-----|--------|
| Vuln-001 | AWS SDK C++ | 1.7.63 | CVE-2025-14760 | Data Integrity Violation |
| Vuln-002 | Boost | 1.70.0 | CVE-2023-52323* | Heap Overflow – RCE |
| Vuln-003 | cURL | 7.63.0 | CVE-2019-3822 | Stack Overflow – RCE |
| Vuln-004 | cURL | 7.63.0 | CVE-2018-16890 | Information Disclosure |
| Vuln-005 | cURL | 7.63.0 | CVE-2021-22890 | MITM Attack |
| Vuln-006 | OpenSSL | 1.1.1b | CVE-2026-41676 | Buffer Overflow – RCE |
| Vuln-007 | OpenSSL | 1.1.1b | CVE-2019-1543 | Nonce Reuse – Info Leak |
| Vuln-008 | OpenSSL | 1.1.1b | CVE-2023-0215 | Use-After-Free – RCE/DoS |
| Vuln-009 | OpenSSL | 1.1.1b | CVE-2023-0286 | Type Confusion – Info Leak |
| Vuln-010 | OpenSSL | 1.1.1b | CVE-2022-0778 | Infinite Loop – DoS |
| Vuln-011 | OpenSSL | 1.1.1b | CVE-2023-0464 | Resource Exhaustion – DoS |
| Vuln-012 | OpenSSL | 1.1.1b | CVE-2023-0465 | Security Bypass |

> ⚠️ **Correction:** CVE-2023-52323 actually belongs to **PyCryptodome**, not Boost. This was a misattribution in the initial report and should be noted accordingly.

---

## 📂 Repository References (Evidence)

The following links provide **direct evidence** of the vulnerable library versions used in Open EDR:

- [AWS SDK C++ 1.7.63 (CHANGELOG.md)](https://github.com/ComodoSecurity/openedr/blob/main/edrav2/eprj/awssdkcpp/CHANGELOG.md)
- [Boost 1.70.0 (version.hpp)](https://github.com/ComodoSecurity/openedr/blob/main/edrav2/eprj/boost/boost/version.hpp)
- [cURL 7.63.0 (RELEASE-NOTES)](https://github.com/ComodoSecurity/openedr/blob/main/edrav2/eprj/curl/RELEASE-NOTES)
- [OpenSSL 1.1.1b (README)](https://github.com/ComodoSecurity/openedr/blob/main/edrav2/eprj/openssl/openssl/README)

---

## 📬 Vendor Communication Timeline

- **June 21, 2026** – Initial report sent to `security@xcitium.com`.
- **June 22, 2026** – Oleg Kalinin (Development Manager CCS) replied: *"We haven't updated for a long time, we will update in the next release."*
- **June 22 – July 10, 2026** – **4 follow-up emails** sent requesting update timeline, CVE assignment, and additional details.
- **July 10, 2026** – **No response** received to any follow-up emails.

> ❗ Xcitium/Comodo has **no official security policy** listed in the repository. There is also no indication whether Open EDR is **actively maintained** or **deprecated**. This creates uncertainty and risk for both users and researchers.

---

## 🧪 PoC / Exploit Status

| CVE | Library | PoC/Exploit Status | References |
|-----|---------|-------------------|------------|
| CVE-2019-3822 | cURL | ✅ Available | [GitHub PoCs](https://cvefeed.io/vuln/detail/CVE-2019-3822) |
| CVE-2018-16890 | cURL | ✅ Available | [GitHub PoC](https://github.com/michelleamesquita/CVE-2018-16890) |
| CVE-2021-22890 | cURL | ✅ Available | [curl.se advisory](https://curl.se/docs/CVE-2021-22890.html) |
| CVE-2025-14760 | AWS SDK | ❌ Not Found | High attack complexity |
| CVE-2023-52323 | Boost* | ❌ Not Found | Misattributed to Boost |
| CVE-2026-41676 | OpenSSL | 🔍 Under Research | New CVE (April 2026) |
| CVE-2019-1543 | OpenSSL | ❌ Not Found | CVSS 0.0 – low priority |
| CVE-2023-0215 | OpenSSL | 🔍 Under Research | No public PoC yet |
| CVE-2023-0286 | OpenSSL | 🔍 Under Research | No public PoC yet |
| CVE-2022-0778 | OpenSSL | 🔍 Under Research | No public PoC yet |
| CVE-2023-0464 | OpenSSL | 🔍 Under Research | No public PoC yet |
| CVE-2023-0465 | OpenSSL | 🔍 Under Research | No public PoC yet |

---

## 📝 Recommendations

### For Open EDR Users:
- **Immediately** review your deployment and consider additional network monitoring.
- Restrict EDR communication to trusted endpoints.
- Evaluate alternative EDR solutions if vendor responsiveness remains poor.

### For Xcitium / Comodo:
- **Urgently** update all third-party libraries.
- Create and publish a **formal security policy**.
- Clearly state the project's maintenance status on GitHub.
- Respond to security reports **professionally and promptly**.

### For the Security Community:
- Treat this as a case study in **supply chain risk**.
- Always audit your dependencies, even those used by your security tools.
- Encourage vendors to adopt **responsible disclosure** practices.

> 🚨 **Critical Takeaway:** An EDR that fails to secure its own dependencies cannot be trusted to secure anything else. Open EDR's reliance on **7-year-old libraries with 100+ CVEs** poses a **direct risk** to every environment where it is deployed.

---

## 📎 Additional Resources

- [MITRE CVE List](https://cve.mitre.org/)
- [cURL Security Advisories](https://curl.se/docs/security.html)
- [OpenSSL Vulnerabilities](https://www.openssl.org/news/vulnerabilities.html)
- [CWE-1104: Use of Unmaintained Third Party Components](https://cwe.mitre.org/data/definitions/1104.html)

---

## ⚖️ Disclaimer

This report is published after a **30-day responsible disclosure period** and **4 follow-up emails** with **no meaningful response** from the vendor. The purpose is to **inform users** and **improve security awareness** in the open-source ecosystem. All findings are based on publicly available information and repository contents.

---

**© 2026 Eneshan Erdoğan Karaca** – This document may be freely shared and referenced with attribution.
