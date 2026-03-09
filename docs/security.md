# Security Operations

## Environment Files

- `.env` is local-only and must remain ignored.
- `.env.example` is tracked and must stay sanitized.
- Use explicit names for secrets: `*_SECRET`, `*_KEY`, `*_TOKEN`.

## Compose Safety Checks

Before merging compose changes, verify:

1. No secret literals are committed.
2. `APP_URL` and other non-secret values are not sourced from secret vars.
3. `docker.sock` is read-only if mounted.
4. Traefik labels use secure routing for external access.
5. Least-privilege controls are present where app compatibility allows.

## CI Secret Scanning

The repository uses `.github/workflows/secret-scan.yml` with Gitleaks.

- Triggered on `pull_request` and `push` to `main`/`master`.
- Fails the workflow on detected leaked secrets.

## Rotation Guidance

When rotating a secret:

1. Update upstream secret manager or deployment environment.
2. Update `.env` on hosts.
3. Restart the affected service.
4. Confirm app health and authentication flows.
