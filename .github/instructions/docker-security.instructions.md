---
applyTo: "**/{docker-compose,docker-compose.*}.{yml,yaml},**/*compose*.{yml,yaml},**/.env*"
description: "Use when editing Docker Compose or env files; enforce secure-by-default container hardening, secret handling, and network exposure controls."
---
# Docker Compose Security Rules

These are hard requirements for all container/runtime changes:

1. Never hardcode secrets. Use environment variable references like `${JWT_SECRET}` and prefer secret files (`*_FILE`) or Docker secrets when available.
2. Do not map `/var/run/docker.sock` unless absolutely required. If required, mount it read-only (`:ro`) and document why.
3. Run services with least privilege: set `user`, add `security_opt: [\"no-new-privileges:true\"]`, and use `cap_drop: [\"ALL\"]` unless a capability is explicitly needed.
4. Bind host ports to localhost by default (for example `127.0.0.1:3552:3552`). Any non-localhost exposure requires explicit justification.
5. Prefer immutable image versions over `:latest` for production stacks.
6. Validate env mappings to prevent secret misuse or leakage (for example, never assign `APP_URL` from a secret variable).
7. Use TLS entrypoints and enable TLS on reverse-proxy routers.

If an exception is unavoidable, document the reason inline in the compose file.

Validation checklist before completing edits:

1. Confirm no secret literals were introduced.
2. Confirm non-secret app settings are not sourced from secret vars.
3. Confirm mounted Docker socket is read-only if used.
4. Confirm exposed routes use TLS labels when publicly reachable.
