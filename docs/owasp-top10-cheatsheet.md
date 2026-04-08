# OWASP Top 10 (2025) — Quick Reference

## A01: Broken Access Control (unchanged)

| Item | Detail |
|------|--------|
| Risk | ผู้ใช้เข้าถึงข้อมูล/function ที่ไม่ได้รับอนุญาต |
| Examples | IDOR, privilege escalation, force browsing, **SSRF** (moved from A10) |
| Fix | Authorization check ทุก request, RBAC, deny by default, allowlist URLs, block internal IPs |

## A02: Security Misconfiguration (was A05)

| Item | Detail |
|------|--------|
| Risk | ตั้งค่าไม่ถูกต้อง ปล่อย default settings เปิด features ไม่จำเป็น |
| Examples | DEBUG=True in prod, default credentials, public S3 buckets, directory listing, missing security headers |
| Fix | Hardening checklist, env-specific configs, automated scanning |

## A03: Software Supply Chain Failures (NEW — was A06 "Vulnerable Components" expanded)

| Item | Detail |
|------|--------|
| Risk | ใช้ library CI/CD pipeline หรือ dependency ที่ถูก compromise |
| Examples | XZ Utils backdoor (CVE-2024-3094), dependency confusion, SolarWinds, Log4Shell, compromised npm/PyPI packages |
| Fix | SBOM generation (Syft), signed builds, lock files, verify checksums, SCA tools (Dependency-Track), private registries |

## A04: Cryptographic Failures (was A02)

| Item | Detail |
|------|--------|
| Risk | การเข้ารหัสไม่เหมาะสม ไม่มี หรือ algorithm ล้าสมัย |
| Examples | Plain text passwords, MD5/SHA1, TLS 1.0/1.1 ยังใช้อยู่, hardcoded keys, weak IV/salt management |
| Fix | bcrypt/argon2, TLS 1.3, AES-256-GCM, ไม่ hardcode, key rotation |

## A05: Injection (was A03)

| Item | Detail |
|------|--------|
| Risk | User input ถูก execute โดยตรงโดยไม่ sanitize |
| Examples | SQL Injection, XSS, Command Injection, **XXE (XML External Entities)** |
| Fix | Parameterized queries, input validation, output encoding, disable DTD in XML parsers |

## A06: Insecure Design (was A04)

| Item | Detail |
|------|--------|
| Risk | ออกแบบระบบไม่ปลอดภัยตั้งแต่แรก |
| Examples | ไม่มี rate limiting, คำถาม security ที่เดาง่าย, ไม่มี threat modeling |
| Fix | Threat modeling, secure design patterns, abuse case testing, rate limiting in design phase |

## A07: Authentication Failures (shortened name from "Identification & Authentication")

| Item | Detail |
|------|--------|
| Risk | ระบบ authentication อ่อนแอ |
| Examples | Weak passwords, no MFA, session ID in URL, **credential stuffing**, weak session management |
| Fix | Strong password policy, MFA, secure sessions, rate limiting, account lockout |

## A08: Software or Data Integrity Failures ("and" -> "or")

| Item | Detail |
|------|--------|
| Risk | ไม่ตรวจสอบ integrity ของ software หรือ data |
| Examples | SolarWinds attack, untrusted libraries, insecure deserialization, unsigned updates |
| Fix | Digital signatures, verify checksums, secure CI/CD, SLSA framework |

## A09: Security Logging and Alerting Failures ("Monitoring" -> "Alerting")

| Item | Detail |
|------|--------|
| Risk | ไม่มี logging/alerting เพียงพอ ตรวจจับ attack ไม่ได้ |
| Examples | ไม่ log failed logins, log sensitive data, ไม่มี alerting, no centralized logging |
| Fix | Log security events, centralized logging (ELK, Loki), real-time alerting, IR plan |

## A10: Mishandling of Exceptional Conditions (NEW — replaces SSRF)

| Item | Detail |
|------|--------|
| Risk | การจัดการ errors/exceptions ไม่ดี ทำให้เกิดช่องโหว่ |
| Examples | Fail-open แทน fail-closed, crash DoS จาก unhandled exception, sensitive data ใน error messages, stack trace เปิดเผย |
| Fix | Fail-closed by default, global exception handlers, sanitize error messages, proper logging not exposing internals |
