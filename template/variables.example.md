# Railway Template Variables

Use these variables in Railway's template composer.

```text
SECURITY_ENABLELOGIN=true
SECURITY_INITIALLOGIN_USERNAME=admin
SECURITY_INITIALLOGIN_PASSWORD=${{ secret(32) }}
SYSTEM_DEFAULTLOCALE=en-US
SYSTEM_GOOGLEVISIBILITY=false
PORT=8080
SERVER_PORT=8080
```

`SECURITY_INITIALLOGIN_PASSWORD` must be generated or supplied at deployment time. Do not commit a real password.

Attach a Railway volume to the Stirling-PDF service at:

```text
/configs
```
