# fair-handler

A standalone Composer plugin that adds FAIR protocol support to any standard Composer installation — no fork required.

## What is FAIR?

FAIR is a decentralized package management protocol built on W3C Decentralized Identifiers (DIDs). Instead of a central registry, packages are discovered by resolving a DID to a metadata document that contains release info, download URLs, checksums, and Ed25519 signatures.

Two DID methods are supported:
- `did:plc:` — resolved via [https://plc.directory/](https://plc.directory/)
- `did:web:` — resolved via HTTPS `did.json` per the W3C spec (e.g. `did:web:example.com`)

## Installation

```bash
composer global require fair/composer-plugin
```

Or as a project dependency:

```bash
composer require fair/composer-plugin
```

## Usage

### Add a FAIR package manually

Add a `fair` repository to your `composer.json`:

```json
{
    "repositories": [
        {
            "type": "fair",
            "packages": {
                "vendor/package-name": "did:plc:abc123"
            }
        }
    ],
    "require": {
        "vendor/package-name": "*"
    }
}
```

### Or use the `fair:require` command

```bash
composer fair:require did:plc:afjf7gsjzsqmgc7dlhb553mv
```

This resolves the DID, fetches the package metadata, writes both the `repositories` entry and the `require` entry to `composer.json`, and runs the install — all in one step.

Options:

```
--vendor=acme      Vendor prefix for the derived package name (default: fair)
--constraint=^1.0  Version constraint (default: *)
--dry-run          Preview changes without writing anything
```

## How it works

The plugin hooks into Composer's repository and download pipeline:

1. **`FairRepository`** (`type: fair`) — on `initialize()`, resolves all configured DIDs (via `PlcDidResolver` or `WebDidResolver`), fetches the metadata document from the service endpoint, and registers Composer `CompletePackage` objects
2. **`Plugin::onPreFileDownload`** — before any download, re-resolves the DID and checks that signing keys and a service endpoint exist
3. **`Plugin::onPostFileDownload`** — after download, verifies the SHA-256 checksum and Ed25519 signature; deletes the file and throws if either check fails

DID documents and metadata are cached to disk (`$COMPOSER_CACHE_DIR/fair/`) to avoid redundant network requests across invocations.

## Relation to the Composer fork

This plugin is a workaround for stock Composer. The [Composer fork](https://github.com/kaigrosz/composer) implements the same functionality directly in Composer core, which allows deeper integration (async DID resolution via `Loop::wait()`, a dedicated `fair-zip` dist type, and a proper `FairDownloader`). The plugin API in this repository is kept as close to that core implementation as possible so the two stay in sync.

## Requirements

- PHP 8.2+
- `ext-sodium`
- `ext-gmp`
- Composer 2.3+

## Running the tests

```bash
composer install
php vendor/bin/phpunit --testdox
```
