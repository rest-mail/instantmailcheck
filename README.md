# instantmailcheck

**Diagnose any mail server in seconds.** Single binary, zero dependencies.

`instantmailcheck` runs a battery of public + authenticated checks against any mail-serving domain — DNS records, SMTP/IMAP/POP3 protocol behavior, TLS, DKIM/SPF/DMARC, MTA-STS, TLS-RPT, and security posture — and tells you what's broken, what's misconfigured, and what's deliverability suicide.

Built and maintained by [rest-mail](https://github.com/rest-mail).

## Install

### Homebrew (macOS / Linux)

```bash
brew install antimatter-studios/tap/instantmailcheck
```

### Go

```bash
go install github.com/rest-mail/instantmailcheck@latest
```

### Pre-built binaries

Download for your platform from the [releases page](https://github.com/rest-mail/instantmailcheck/releases) — single binary, no installer.

## Usage

Four tiers of checks, layered. Run only what you need.

### Tier 1 — public audit (no credentials)

```bash
instantmailcheck example.com
```

Checks DNS (MX, SPF, DKIM, DMARC, MTA-STS, TLS-RPT), reachability of advertised mail servers, TLS cert validity + chain, STARTTLS support, banner/EHLO behavior. Everything an outsider can see.

```bash
instantmailcheck example.com --dkim-selector default
```

Specify a DKIM selector if you know it (otherwise the tool tries common selectors).

### Tier 2 — send a test message

```bash
instantmailcheck example.com --send-to alice@example.com
```

Delivers a test message and reports the SMTP dialogue, accept/defer/reject behavior, and round-trip latency. Useful for verifying inbound mail acceptance from outside.

### Tier 3 — authenticated round-trip

```bash
instantmailcheck example.com \
  --user alice@example.com \
  --pass 'hunter2' \
  --send-to alice@example.com
```

Logs in via SMTP submission, sends a message, then logs in via IMAP/POP3 and verifies the message arrives and is retrievable. End-to-end deliverability check from a real account's perspective.

### Tier 4 — security audit

```bash
instantmailcheck example.com --security-audit
```

Safe, non-destructive exploit-simulation checks: open relay attempts, AUTH-without-TLS detection, downgrade behavior, header injection probes. Adds the security posture grade to the report.

## Output formats

- **Default** — colored, structured terminal output for humans
- `--json` — machine-readable for scripting and CI
- `--markdown` — for pasting into tickets, PRs, runbooks
- `--output report.txt` — write to file instead of stdout
- `--checks <pattern>` — filter to a category (e.g. `dns`, `tls`, `security`)
- `--threshold <N>` — exit code 0 if score ≥ N% (default 50), non-zero otherwise — use in CI gates

## Exit codes

| Code | Meaning |
|------|---------|
| 0    | All checks passed (or score ≥ threshold) |
| 1    | One or more checks failed below threshold |
| 2    | Tool error (unreachable, invalid args) |

## Why use this

Most mail-server diagnosis happens with `dig` + `swaks` + `openssl s_client` + 30 minutes of manual investigation. `instantmailcheck` is one command that runs all of that and tells you the answer.

Suitable for:
- **Migration verification** — before cutting MX records, prove the new server actually works.
- **CI / monitoring** — gate deploys on `--threshold 80`, alert on score drops.
- **Audit / compliance** — point it at a domain, get a Markdown report.
- **Triaging deliverability complaints** — "is mail actually broken, or just slow?" answered in one command.

## License

MIT.
