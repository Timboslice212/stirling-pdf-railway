# Stirling-PDF on Railway

Railway deployment template for [Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF), the self-hosted PDF toolkit.

This repository does not fork or repackage Stirling-PDF source code. It provides a small Railway-focused wrapper around the official Docker image, with pinned versioning, authentication guidance, persistence notes, health checks, upgrade instructions, and acceptance tests.

## What Gets Deployed

- One public Railway service running `stirlingtools/stirling-pdf:2.14.3`
- One persistent Railway volume mounted at `/configs`
- Login enabled by default
- Public HTTPS networking for the web UI and upstream API
- Railway startup health check against `/login`

No custom PDF engine, database service, queue, analytics service, or external OCR/API provider is added.

## Upstream Version

Tested target:

```text
stirlingtools/stirling-pdf:2.14.3
```

`2.14.3` is the latest stable upstream release identified during the September 8, 2026 audit. The tag is pinned instead of using `latest` so deploys are repeatable.

## Why Railway

Stirling-PDF already runs well in Docker. Railway adds a managed deployment surface: HTTPS routing, environment variables, logs, persistent volumes, redeploys, and a template flow that makes the setup easier to repeat.

## Required Railway Settings

Configure the Railway service as follows:

- Source: this repository
- Builder: Dockerfile
- Public networking: enabled
- Port: `8080`
- Health check path: `/login`
- Health check timeout: `600`
- Volume mount path: `/configs`

Template variables:

```text
SECURITY_ENABLELOGIN=true
SECURITY_INITIALLOGIN_USERNAME=admin
SECURITY_INITIALLOGIN_PASSWORD=${{ secret(32) }}
SYSTEM_DEFAULTLOCALE=en-US
SYSTEM_GOOGLEVISIBILITY=false
```

Do not hard-code a real password in this repository. In Railway's template composer, use `secret(32)` or require the deployer to provide a password.

## First Login

Open the Railway public URL after the deployment is healthy.

- Username: value of `SECURITY_INITIALLOGIN_USERNAME`
- Password: value generated or supplied for `SECURITY_INITIALLOGIN_PASSWORD`

Change the admin password after first login. Store credentials in a password manager. Do not place credentials in GitHub, the README, or support tickets.

## Persistence

The Railway volume is mounted at `/configs`.

According to upstream documentation, `/configs` stores settings and the application database. This is the most important persistent state for users, authentication, and app configuration.

Temporary uploaded and processed files should be treated as ephemeral. Railway volume storage is persistence, not a complete disaster-recovery backup strategy. Back up the volume before upgrades and before changing authentication or signing-related settings.

## Features

This template exposes upstream Stirling-PDF features available in the selected official image, including PDF merge, split, compression, conversion, OCR, signing, redaction, pipelines, and the REST API.

Feature availability can vary by upstream image variant and release. This template uses the standard image. If your workload needs additional fonts or tooling for high-fidelity Office conversions, test `2.14.3-fat` in a staging deployment before switching.

## API

Stirling-PDF provides the upstream API and Swagger/OpenAPI UI. This template does not add a custom API layer.

After deployment, create and rotate API keys in the Stirling-PDF UI. Treat API keys like passwords. Never commit them.

## Health Check

This template uses `/login` as the Railway health check path because login is enabled by default and prior upstream discussions indicate `/api/v1/info/status` may be disabled or unsuitable for unauthenticated health checks in v2.

## Upgrades

1. Back up the Railway volume.
2. Change the pinned image tag in `Dockerfile`.
3. Run the local smoke checks where Docker is available.
4. Deploy to a Railway test environment.
5. Verify login, health check, PDF operations, OCR, Office conversion, API access, and persistence.
6. Promote the change only after validation.

Rollback is the reverse: restore the previous image tag and redeploy. If an upgrade changed persistent data, restore the pre-upgrade backup before rolling back.

## Licensing

This repository is a Railway packaging layer and does not distribute upstream Stirling-PDF source code.

The upstream root license is MIT with explicit carve-outs for several directories. The upstream `engine/` directory is governed by Stirling's User License, which includes production-use restrictions. Because the official Docker image may include components covered by those boundaries, commercial Railway Marketplace publication should receive human/legal review before publishing.

Do not imply this template is official or endorsed by Stirling PDF Inc. unless that approval is obtained separately.

## Documentation

- [Architecture](docs/architecture.md)
- [Security](docs/security.md)
- [Upgrades](docs/upgrades.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Acceptance checklist](tests/acceptance-checklist.md)
- [Smoke test](tests/smoke-test.md)
