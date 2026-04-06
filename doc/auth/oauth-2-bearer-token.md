
# OAuth 2 Bearer token



Documentation for accessing and setting credentials for OAuthBearerToken.

## Auth Credentials

| Name | Type | Description | Setter | Getter |
|  --- | --- | --- | --- | --- |
| AccessToken | `string` | The OAuth 2.0 Access Token to use for API requests. | `accessToken` | `getAccessToken()` |



**Note:** Auth credentials can be set using `OAuthBearerTokenCredentialsBuilder::init()` in `oAuthBearerTokenCredentials` method in the client builder and accessed through `getOAuthBearerTokenCredentials` method in the client instance.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```php
use MultiAuthSampleLib\Authentication\OAuthBearerTokenCredentialsBuilder;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->oAuthBearerTokenCredentials(
        OAuthBearerTokenCredentialsBuilder::init(
            'AccessToken'
        )
    )
    ->build();
```


