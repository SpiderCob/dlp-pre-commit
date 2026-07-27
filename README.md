# dlp-pre-commit

Pre-commit hooks for [Spidercob DLP](https://spidercob.com) — block commits that contain API keys, passwords, AWS credentials, and 40+ other secret and PII patterns.

**No signup required for secrets-only scanning.** Full PII scanning requires a free Spidercob account.

## Setup

```bash
pip install pre-commit
pre-commit install
```

Add to `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/SpiderCob/dlp-pre-commit
    rev: v1.0.0
    hooks:
      - id: dlp-scan-secrets-only
```

## Available hooks

| Hook ID | What it scans | Account needed |
|---|---|---|
| `dlp-scan-secrets-only` | API keys, AWS credentials, tokens, passwords, DB strings, 40+ patterns | No — fully offline |
| `dlp-scan-critical-only` | CRITICAL severity only — high-confidence keys and tokens | No — fully offline |
| `dlp-scan-full` | PII (SSN, CC, email, phone) + all secrets | Yes — requires `SPIDERCOB_TOKEN` |

## Examples

### Secrets only (recommended, zero config)

```yaml
repos:
  - repo: https://github.com/SpiderCob/dlp-pre-commit
    rev: v1.0.0
    hooks:
      - id: dlp-scan-secrets-only
```

### Full PII + secrets scan (requires token)

```yaml
repos:
  - repo: https://github.com/SpiderCob/dlp-pre-commit
    rev: v1.0.0
    hooks:
      - id: dlp-scan-full
        env:
          SPIDERCOB_TOKEN: sk_live_...
```

### Scan specific file types only

```yaml
repos:
  - repo: https://github.com/SpiderCob/dlp-pre-commit
    rev: v1.0.0
    hooks:
      - id: dlp-scan-secrets-only
        types: [python, javascript, yaml]
```

## What gets detected

**Secrets (offline, no account):**
- AWS access keys and secret keys
- GitHub / GitLab personal access tokens
- Stripe, Twilio, SendGrid API keys
- Private RSA/EC keys
- Database connection strings (Postgres, MySQL, MongoDB)
- JWT tokens
- Generic high-entropy strings likely to be secrets

**PII (requires Spidercob account):**
- Social Security Numbers
- Credit card numbers (PCI)
- Email addresses in sensitive context
- Phone numbers
- Passport / driver's license numbers

## Get a token

Sign up free at [spidercob.com](https://spidercob.com/register).
