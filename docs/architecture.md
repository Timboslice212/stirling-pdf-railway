# Architecture

## Decision

Use a single Railway service running the official Stirling-PDF Docker image:

```text
docker.stirlingpdf.com/stirlingtools/stirling-pdf:2.14.3
```

This is the simplest architecture that matches the current upstream Docker guidance. The standard image includes the main Stirling-PDF feature set, including local PDF processing, OCR support, conversion tooling, authentication, the web UI, and the upstream API.

## Railway Resources

- Public service: `stirling-pdf`
- Docker source: this repository's `Dockerfile`
- Public HTTPS: enabled
- Health check: `/login`
- Persistent volume: mounted at `/configs`

## Why Not Add Unoserver By Default

A separate private LibreOffice/unoserver service adds operational complexity, startup ordering, networking, and failure modes. The current upstream Docker distribution provides conversion functionality in the standard and fat image variants, so this template starts with one service.

If a deployment has high-volume or high-fidelity Office conversion needs, test `docker.stirlingpdf.com/stirlingtools/stirling-pdf:2.14.3-fat` first. Add a private conversion worker only if a real Railway validation shows the single-service image is insufficient.

## Persistence

The `/configs` mount is the primary persistent path. Upstream documentation identifies it as the location for settings and the application database.

The following paths are useful in broader Docker deployments but are not mounted by default here:

- `/usr/share/tessdata` for additional OCR language data
- `/logs` for application logs
- `/pipeline` for automation configurations
- `/customFiles` for customization assets

Railway logs already capture runtime output. If a deployment relies heavily on custom OCR languages, pipelines, or custom files, evaluate additional Railway volumes or a custom image in a staging project.
