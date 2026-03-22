# FAIR Composer Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PHP Version](https://img.shields.io/badge/PHP-%5E8.2-8892BF.svg)](composer.json)

A Composer plugin that adds support for the [FAIR decentralized package management protocol](https://fair.pm). Install packages using [Decentralized Identifiers (DIDs)](https://www.w3.org/TR/did-core/) instead of relying on a central registry — every download is cryptographically verified with Ed25519 signatures and SHA-256 checksums.

## Quick Start

```bash
composer require fair/composer-plugin

composer fair:require did:plc:abc123
```

That's it. The plugin resolves the DID, fetches package metadata from the publisher's own service endpoint, and verifies every download before it touches your project.

## How It Works

1. **DID Resolution** — The plugin resolves `did:plc:` (via [PLC Directory](https://plc.directory/)) and `did:web:` (via HTTPS `did.json`) identifiers to locate the publisher's signing keys and service endpoint.
2. **Metadata Fetch** — Package metadata (versions, artifacts, checksums, signatures) is fetched directly from the publisher's FAIR service endpoint.
3. **Pre-Download Validation** — Before any HTTP download begins, the plugin verifies the DID resolves correctly, has signing keys, and has a valid service endpoint. Invalid packages are blocked entirely.
4. **Post-Download Verification** — After download, the file's SHA-256 checksum and Ed25519 signature are verified against the DID document's keys. Unverified files are deleted from disk immediately.

## Installation

```bash
composer require fair/composer-plugin
```

**Requirements:**
- PHP 8.2+
- `ext-sodium` (for Ed25519 signature verification)
- `ext-gmp` (for key decoding)
- Composer 2.3+

## Usage

### Require a FAIR package by DID

```bash
composer fair:require did:plc:abc123
```

Options:
- `--vendor` — Vendor prefix for the package name (default: `fair`)
- `--constraint` — Version constraint (default: `*`)
- `--dry-run` — Show what would be done without writing changes

### Manual configuration

Add a FAIR repository to your `composer.json`:

```json
{
    "repositories": [
        {
            "type": "fair",
            "packages": {
                "vendor/package-name": "did:plc:abc123"
            }
        }
    ]
}
```

Or use DID-only config with an optional vendor prefix:

```json
{
    "repositories": [
        {
            "type": "fair",
            "vendor": "my-vendor",
            "dids": ["did:plc:abc123", "did:web:example.com"]
        }
    ]
}
```

## Supported DID Methods

| Method | Resolution | Example |
|--------|-----------|---------|
| `did:plc:` | [PLC Directory](https://plc.directory/) | `did:plc:abc123def456` |
| `did:web:` | HTTPS `did.json` (W3C spec) | `did:web:example.com` |

## Development

```bash
git clone https://github.com/KaiGrosz/fair-handler.git
cd fair-handler
composer install
composer test
```

### Running tests

```bash
vendor/bin/phpunit
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for development setup and guidelines.

## Security

If you discover a security vulnerability, please see [SECURITY.md](SECURITY.md) for responsible disclosure instructions. **Do not open a public issue.**

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
