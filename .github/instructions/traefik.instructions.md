---
applyTo: "**/{docker-compose,docker-compose.*}.{yml,yaml},**/*compose*.{yml,yaml}"
description: "Use when adding or editing Traefik labels in compose files; enforce TLS-first routing, explicit router/service labels, and safe exposure defaults."
---
# Traefik Label Rules

These are hard requirements for Traefik labels:

1. Enable Traefik explicitly with `traefik.enable=true` only for services intended to be exposed.
2. Routers must use secure entrypoints (`websecure`) and set `traefik.http.routers.<name>.tls=true`.
3. Every router must define a strict `Host(...)` rule; avoid wildcard host rules unless explicitly required.
4. Service port labels must be explicit: `traefik.http.services.<name>.loadbalancer.server.port=<internal-port>`.
5. Reference the expected Docker network with `traefik.docker.network=<network>`.
6. Security middleware should be included by default (authentication, headers) when available in the deployment.
7. Do not expose admin/internal-only services through Traefik unless there is a documented operational need.

Validation checklist before completing edits:

1. Router labels include secure entrypoint and TLS.
2. Host rule is explicit and scoped.
3. Service port and docker network labels are present.
