# Security Policy

## Secret Handling

This repository must not contain:

- API keys
- OAuth tokens
- n8n credential exports
- `.env` files
- private Google Sheet URLs
- private Google Drive file URLs
- production email account secrets
- service account JSON files

All secrets should remain in n8n credential storage or another dedicated secret manager.

## Before Publishing

Run a text scan for common secret terms:

```powershell
rg -n "(?i)(api[_-]?key|secret|token|password|credential|oauth|bearer)" .
```

Review binary files manually before publishing if they came from a live system.

## Reporting

If a secret is accidentally committed, rotate the secret immediately and remove it from git history before making the repository public.
