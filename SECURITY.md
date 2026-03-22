# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| latest  | Yes       |

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please report it responsibly.

**Do not open a public GitHub issue for security vulnerabilities.**

Instead, please use [GitHub Private Vulnerability Reporting](https://github.com/KaiGrosz/fair-handler/security/advisories/new) to submit your report.

### What to include

- A description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if you have one)

### Response timeline

- **Acknowledgment** — within 48 hours
- **Initial assessment** — within 1 week
- **Fix or mitigation** — as soon as reasonably possible, depending on severity

## Security Scope

This plugin handles cryptographic operations (Ed25519 signature verification, SHA-256 checksums) and network requests (DID resolution, metadata fetching). Issues in these areas are especially relevant.
