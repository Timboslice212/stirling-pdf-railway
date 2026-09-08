# Security

## Defaults

Login is enabled by default:

```text
SECURITY_ENABLELOGIN=true
```

The template should also set:

```text
SECURITY_INITIALLOGIN_USERNAME=admin
SECURITY_INITIALLOGIN_PASSWORD=${{ secret(32) }}
SYSTEM_GOOGLEVISIBILITY=false
```

Use Railway variables for all secrets. Do not commit real credentials.

## Public URL

Railway public networking exposes the application on the internet. Anyone with the URL can reach the login page. Use a strong admin password, rotate credentials when staff change, and disable public networking if the deployment is meant to be private.

## API Keys

Create API keys inside Stirling-PDF after login. Store them in a password manager or Railway variables for downstream services. Do not place API keys in documentation, examples, screenshots, or Git history.

## Password Rotation

Rotate the admin password from the application UI. If the initial password variable is changed after the first run, confirm upstream behavior in a test deployment before assuming it resets existing users.

## Backups

Back up `/configs` before upgrades and before security-sensitive changes. The volume may contain users, authentication state, settings, and application database files.

## Licensing Gate

The upstream root license is MIT with carve-outs. The upstream `engine/` directory is governed by Stirling's User License, which includes production-use restrictions. Because this template references the official image rather than distributing upstream source, repository distribution is lower risk than copying source, but Marketplace publication still needs human/legal review.
