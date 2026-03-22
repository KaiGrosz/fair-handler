# Contributing to FAIR Composer Plugin

Thanks for your interest in contributing! This document explains how to get started.

## Development Setup

1. Fork and clone the repository
2. Install dependencies:

```bash
composer install
```

3. Run the test suite to verify everything works:

```bash
vendor/bin/phpunit
```

## Requirements

- PHP 8.2+
- `ext-sodium` and `ext-gmp`
- Composer 2.3+

## Code Style

- Use `declare(strict_types=1)` in all PHP files
- Follow PSR-4 autoloading (`Fair\ComposerPlugin\` namespace maps to `src/`)
- Use `final` classes unless inheritance is explicitly needed
- Type all parameters, return types, and properties

## Running Tests

```bash
# Run all tests
vendor/bin/phpunit

# Run a specific test file
vendor/bin/phpunit tests/Unit/Security/KeyDecoderTest.php
```

Tests live in `tests/Unit/` and mirror the `src/` directory structure.

## Pull Request Process

1. Create a feature branch from `main`
2. Make your changes with clear, focused commits
3. Ensure all tests pass
4. Open a pull request against `main` with a clear description of what changed and why
5. Link any related issues using `Closes #123`

## Reporting Bugs

Use the [bug report template](https://github.com/KaiGrosz/fair-handler/issues/new?template=01-bug-report.yml) to file issues. Include steps to reproduce, expected behavior, and your environment details.

## Security Issues

**Do not open a public issue for security vulnerabilities.** See [SECURITY.md](SECURITY.md) for responsible disclosure instructions.
