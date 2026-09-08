# Troubleshooting

## Deployment Fails

Check Railway build logs first. This template uses a Dockerfile that references the official pinned image, so common causes are image pull problems, Docker registry availability, or Railway source configuration.

## Health Check Fails

Confirm:

- `PORT=8080`
- `SERVER_PORT=8080`
- health check path is `/login`
- health check timeout is at least `600`
- the service has enough memory for startup

Do not use `/api/v1/info/status` as the default health check without validating it on the selected version.

## Login Fails

Confirm the Railway variables:

```text
SECURITY_ENABLELOGIN=true
SECURITY_INITIALLOGIN_USERNAME=admin
SECURITY_INITIALLOGIN_PASSWORD=<secret>
```

If the app has already initialized users in `/configs`, changing the initial login variables may not update an existing admin password. Use the UI or documented upstream recovery process.

## Data Does Not Persist

Confirm the Railway volume is mounted at `/configs`. Settings and the application database should live there. If the service was deployed once without a volume, create a fresh deployment after attaching the volume or migrate state intentionally.

## OCR Problems

English OCR should work with the standard image. Additional languages may need language data. For production use beyond the bundled languages, test a custom image or persistent OCR language strategy in staging.

## Office Conversion Problems

The standard image is the default. If DOCX, XLSX, or PPTX conversion quality is not acceptable, test `stirlingtools/stirling-pdf:2.14.3-fat` in staging. Add a private conversion worker only after confirming the single-service image cannot satisfy the workload.

## Large Files Fail

Large PDF, OCR, and Office conversion workloads can require more memory, CPU, and temporary disk. Increase Railway resources and test with representative documents.
