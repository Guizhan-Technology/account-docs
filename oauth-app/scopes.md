# Scopes for OAuth application

Scopes let you specify exactly what type of access you need. Scopes limit access for OAuth tokens. They do not grant any additional permission beyond that which the user already has.

---

In the [Authorization flow](/oauth-app/authorize?id=authorization-redirect), request scopes are displayed to users on authorization page.

## Available scopes :id=scopes

|Name|Description|
|-|-|
|(No scopes)|`user:read` is used as the default scope.|
|user:read|*Default scope*. Read the user's general information (excluding email address).|
|user:email|Read the user's email address.|