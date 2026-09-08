# Upgrades

## Current Version

```text
stirlingtools/stirling-pdf:2.14.3
```

## Upgrade Process

1. Read the upstream release notes.
2. Check upstream security advisories.
3. Back up the Railway `/configs` volume.
4. Update the image tag in `Dockerfile`.
5. Run the smoke test locally where Docker is available.
6. Deploy to a Railway staging or test environment.
7. Verify login, health check, representative PDF tools, OCR, Office conversion, API, restart, redeploy, and persistence.
8. Promote only after validation.

## Rollback

1. Restore the previous image tag in `Dockerfile`.
2. Redeploy.
3. If the newer version migrated persistent state in a backward-incompatible way, restore the pre-upgrade `/configs` backup.

Do not roll back blindly after database or settings migrations.
