# Arcane Compose Stack

This repository contains a Docker Compose deployment for Arcane with Traefik integration and secret-hygiene guardrails.

## Files

- `docker-compose.yml`: Runtime stack definition.
- `.env.example`: Safe template for local environment values.
- `.gitignore`: Prevents accidental env secret commits.
- `.github/workflows/secret-scan.yml`: CI secret scanning with Gitleaks.
- `.github/instructions/*.instructions.md`: Agent rules for secure edits.

## Quick Start

1. Copy env template:
   - `cp .env.example .env`
2. Fill real secret values in `.env`.
3. Start services:
   - `docker compose up -d`
4. Check logs:
   - `docker logs arcane`

## Security Baseline

1. Keep real secrets in `.env` only; never commit them.
2. Keep `docker.sock` read-only if mounted.
3. Prefer Traefik TLS routing (`websecure` + `tls=true`) for internet-exposed services.
4. Keep least privilege defaults (`cap_drop: [ALL]`, and when compatible, `no-new-privileges:true`).
5. Keep automated scanning enabled in CI.

## Secret Leak Recovery

If a secret is committed by mistake:

1. Rotate the secret in the upstream system immediately.
2. Replace committed literal values with env references.
3. Remove the file from git index if needed:
   - `git rm --cached .env`
4. Keep `.env` ignored and `.env.example` sanitized.

## Agent Instructions

Security instructions live in `.github/instructions/`:

- `docker-security.instructions.md`
- `secrets.instructions.md`
- `secret-leak-prevention.instructions.md`
- `traefik.instructions.md`

These are hard rules for compose, env, and workflow edits.
