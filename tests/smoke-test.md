# Smoke Test

Run these checks before publishing or promoting a deployment.

## Local

Start the image with a local `/configs` directory and login enabled.

Expected results:

- Container starts.
- `GET /api/v1/info/status` returns `200` and `status: UP`.
- Login page loads.
- Admin login works with the configured initial credentials.
- A simple PDF operation succeeds.
- A setting change survives container restart.

## Railway

Expected results:

- Deployment succeeds.
- Health check passes against `/api/v1/info/status`.
- Public HTTPS URL loads.
- Login works.
- Logs do not contain secret values.
- `/configs` volume is attached.
- A setting change survives restart.
- A setting change survives redeploy.
