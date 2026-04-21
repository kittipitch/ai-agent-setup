---
name: security-scan-list
description: >
  Performs a deep-dive security audit based on Session 4 criteria (CWE/OWASP).
  Triggered when user asks to review, audit, or check project security.
  Keywords: security, audit, vulnerability, scan, review, owasp, cwe
allowed-tools:
  - Read
  - Grep
  - Bash
---

# security-scan-list Skill

Review this code for security vulnerabilities. Check specifically for:
1. SQL injection (string concatenation in queries) — CWE-89
2. Hardcoded secrets (API keys, passwords) — CWE-798
3. XSS vulnerabilities (unescaped output) — CWE-79
4. Missing authentication checks — CWE-306
5. Debug endpoints exposing internal info — CWE-215
6. Regex injection (ReDoS) — CWE-1333
7. Information disclosure in errors — CWE-209
8. Missing input validation — CWE-20
9. No enum validation on constrained fields — CWE-20
10. Plaintext passwords (stored without hashing) — CWE-307

For each finding:
- Location: File:Line
- Severity: Critical / High / Medium / Low
- CWE/OWASP mapping
- Suggested fix: How to remediate?

Output format:
## Security Audit Report
| # | Finding | Location | Severity | CWE |
|---|---------|----------|----------|-----|
| 1 | ...     | ...      | ...      | ... |

After the table, provide a prioritized fix plan:
1. Fix Critical items first
2. Then High
3. Then Medium/Low
