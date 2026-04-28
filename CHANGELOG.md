# Changelog

Semver. Newest on top.

## v0.1.0 (2026-04-28)

Initial release. Extracted from rest-mail-server into a standalone repo.

- Tier 1 — public audit: DNS (MX/SPF/DKIM/DMARC/MTA-STS/TLS-RPT), reachability, TLS, STARTTLS, banner/EHLO
- Tier 2 — send-only test
- Tier 3 — authenticated round-trip (submit + retrieve)
- Tier 4 — security audit (open relay, downgrade, AUTH-without-TLS, header injection)
- Output: terminal, `--json`, `--markdown`, `--output <file>`
- `--checks <pattern>` filter, `--threshold <N>` for CI gates
- Cross-platform binaries (linux/darwin/windows × amd64/arm64)
- Homebrew formula at `antimatter-studios/tap/instantmailcheck`
