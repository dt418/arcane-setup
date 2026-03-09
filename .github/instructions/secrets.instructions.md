---
applyTo: "**/.env*,**/*.{env,env.example},**/{docker-compose,docker-compose.*}.{yml,yaml},**/*compose*.{yml,yaml}"
description: "Use when handling env values, docker compose env blocks, or app secrets; enforce secure secret storage, naming, and rotation-safe patterns."
---
# Secrets Handling Rules

These are hard requirements:

1. Never commit real secrets to git. Keep real values in local `.env` only and maintain sanitized templates in `.env.example`.
2. Never place secret literals directly in compose files. Use variable references like `${JWT_SECRET}`.
3. Use explicit secret variable names (`*_SECRET`, `*_KEY`, `*_TOKEN`) and avoid ambiguous names.
4. Do not map non-secret settings from secret variables (for example, never set `APP_URL` from `JWT_SECRET`).
5. Prefer file-based secret injection (`*_FILE`) or Docker secrets if supported by the container image.
6. Any new secret must include a rotation note in comments or docs (where it is consumed and how to replace it safely).
7. Keep `.env.example` non-sensitive and structurally complete so deployments can be configured without exposing credentials.

Validation checklist before completing edits:

1. `.env` remains ignored and untracked.
2. `.env.example` contains placeholders, not real values.
3. New secrets have clear names and rotation notes.
