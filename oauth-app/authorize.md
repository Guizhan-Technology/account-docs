# Authorizing OAuth application

You can enable other users to authorize your OAuth application.

Guizhan Account's OAuth implementation supports the standard [authorization code grant type](https://tools.ietf.org/html/rfc6749#section-4.1).

## Authorization flow :id=authorization

The authorization flow for application users is：

1. Users are redirected from application to Guizhan Account, to request authorization by users

2. Users are redirected back to application by Guizhan Account

3. The application accesses the API with user's access token

### 1. Redirect to Guizhan Account :id=authorization-redirect

```
GET https://account.guizhanss.com/oauth/authorize
```

When a user redirected from application, it will prompts the user to sign in with an account and authorize the application.

*Parameters*

|Name|Type|Description|
|-|-|-|
|client_id|`string`|**Required**. The **Client ID** you received when you [created an application](/oauth-app/create).|
|response_type|`string`|**Required**. It should be `code` here.|
|redirect_uri|`string`|**Required**. It needs to be exactly the same as the callback URL in the application settings.|
|scope|`string`|a comma-delimited list of [scopes](/oauth-app/scopes?id=scopes). If not provided, the default scopes are used.|
|state|`string`|An random string. It is used to protect against cross-site request forgery attacks.|

### 2. Users are redirected back to application :id=authorization-callback

If the user accepts your request, Guizhan Account redirects user back to your application with the parameters `code` and `state`(if possible).

Validate the `state`, and send a POST request to exchange this code for access token.

```
POST https://account.guizhanss.com/oauth/token
```

*Parameters*

|Name|Type|Description|
|-|-|-|
|client_id|`string`|**Required**. The **Client ID** you received when you [created an application](/oauth-app/create).|
|client_secret|`string`|**Required**. The **Client Secret** you received when you [created an application](/oauth-app/create).|
|grant_type|`string`|**Required**. It should be`authorization_code`。|
|redirect_uri|`string`|**Required**. It needs to be exactly the same as the callback URL in the application settings.|
|code|`string`|**Required**. The `code` parameter received from previous request.|

*Response*

Guizhan Account will return a JSON response containing `access_token`, `refresh_token` and `expires_in` attributes.

```json
{
    "token_type": "Bearer",
    "expires_in": 31536000,
    "access_token": "response access_token",
    "refresh_token": "response refresh_token"
}
```

### 3. Use the access token to access the API :id=authorization-api

The access token allows you to make requests to the API using the user's identity.

```
GET https://api.account.guizhanss.com/user
Authorization: Bearer access_token
```

## FAQ :id=faq

### An error page is displayed after redirecting to Guizhan Account

There are many reasons for the error page to be displayed. Please make sure:

- Your account is in normal status.
- The application is in normal status.
- Every parameter is provided correctly according to the documents.
    - `redirect_uri` needs to be **exactly the same** as the callback URL in the application settings.
    - `scope` list is delimited by commas `,`. Only list the scopes displayed in [Available scopes](/oauth-app/scopes?id=scopes).