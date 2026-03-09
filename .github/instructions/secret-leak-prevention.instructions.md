---
applyTo: "**/.gitignore,**/.env*,**/.github/workflows/*.{yml,yaml},**/{docker-compose,docker-compose.*}.{yml,yaml},**/*compose*.{yml,yaml}"
description: "Use when editing gitignore, CI workflows, env files, or compose files; enforce controls that prevent accidental secret disclosure."
---
# Secret Leak Prevention Rules

These are hard requirements:

1. Ensure `.env` and `.env.*` are ignored by git, with only `.env.example` allowed for tracked templates.
2. CI workflows must not print secret values or dump full environments to logs.
3. Use masked secrets from CI providers (for example GitHub Actions `${{ secrets.NAME }}`) and avoid plaintext tokens in workflow YAML.
4. Do not commit generated keys, tokens, or cert private keys to the repository.
5. If a potential secret is discovered in a tracked file, stop and replace it with a variable reference, then rotate the secret out-of-band.
6. Prefer automated secret scanning in CI (for example Gitleaks or equivalent) on pull requests.
7. Keep example configs usable but sanitized so no real endpoints, usernames, or credentials are leaked.

Validation checklist before completing edits:

1. Workflow steps do not print secrets.
2. `.gitignore` still blocks `.env` and `.env.*`.
3. Secret scanning remains enabled for PRs.
