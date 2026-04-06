
# OAuth 2 Authorization Code Grant



Documentation for accessing and setting credentials for OAuthACG.

## Auth Credentials

| Name | Type | Description | Setter | Getter |
|  --- | --- | --- | --- | --- |
| OAuthClientId | `string` | OAuth 2 Client ID | `oAuthClientId` | `getOAuthClientId()` |
| OAuthClientSecret | `string` | OAuth 2 Client Secret | `oAuthClientSecret` | `getOAuthClientSecret()` |
| OAuthRedirectUri | `string` | OAuth 2 Redirection endpoint or Callback Uri | `oAuthRedirectUri` | `getOAuthRedirectUri()` |
| OAuthToken | `OAuthToken\|null` | Object for storing information about the OAuth token | `oAuthToken` | `getOAuthToken()` |
| OAuthScopes | `string[]\|null` | List of scopes that apply to the OAuth token | `oAuthScopes` | `getOAuthScopes()` |
| OAuthClockSkew | `int` | Clock skew time in seconds applied while checking the OAuth Token expiry. | `oAuthClockSkew` | - |



**Note:** Auth credentials can be set using `OAuthACGCredentialsBuilder::init()` in `oAuthACGCredentials` method in the client builder and accessed through `getOAuthACGCredentials` method in the client instance.

## Usage Example

### 1\. Client Initialization

You must initialize the client with *OAuth 2.0 Authorization Code Grant* credentials as shown in the following code snippet.

```php
use MultiAuthSampleLib\Authentication\OAuthACGCredentialsBuilder;
use MultiAuthSampleLib\Models\OAuthScopeOAuthACGEnum;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->oAuthACGCredentials(
        OAuthACGCredentialsBuilder::init(
            'OAuthClientId',
            'OAuthClientSecret',
            'OAuthRedirectUri'
        )
            ->oAuthScopes(
                [
                    OAuthScopeOAuthACGEnum::READ_SCOPE
                ]
            )
    )
    ->build();
```



Your application must obtain user authorization before it can execute an endpoint call in case this SDK chooses to use *OAuth 2.0 Authorization Code Grant*. This authorization includes the following steps

### 2\. Obtain user consent

To obtain user's consent, you must redirect the user to the authorization page.The `buildAuthorizationUrl()` method creates the URL to the authorization page. You must have initialized the client with scopes for which you need permission to access.

```php
$authUrl = $client->getOAuthACGCredentials()->buildAuthorizationUrl();
header('Location: ' . filter_var($authUrl, FILTER_SANITIZE_URL));
```

### 3\. Handle the OAuth server response

Once the user responds to the consent request, the OAuth 2.0 server responds to your application's access request by redirecting the user to the redirect URI specified set in `Configuration`.

If the user approves the request, the authorization code will be sent as the `code` query string:

```
https://example.com/oauth/callback?code=XXXXXXXXXXXXXXXXXXXXXXXXX
```

If the user does not approve the request, the response contains an `error` query string:

```
https://example.com/oauth/callback?error=access_denied
```

### 4\. Authorize the client using the code

After the server receives the code, it can exchange this for an *access token*. The access token is an object containing information for authorizing client requests and refreshing the token itself.

```php
try {
    $token = $client->getOAuthACGCredentials()->fetchToken($_GET['code']);
    // re-build the client with oauth token
    $client = $client
        ->toBuilder()
        ->oAuthACGCredentials($client->getOAuthACGCredentialsBuilder()->oAuthToken($token))
        ->build();
} catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
    // handle exception
}
```

### Scopes

Scopes enable your application to only request access to the resources it needs while enabling users to control the amount of access they grant to your application. Available scopes are defined in the [`OAuthScopeOAuthACGEnum`](../../doc/models/o-auth-scope-o-auth-acg-enum.md) enumeration.

| Scope Name | Description |
|  --- | --- |
| `READ_SCOPE` | Read request for files |

### Refreshing the token

An access token may expire after sometime. To extend its lifetime, you must refresh the token.

```php
if ($client->getOAuthACGCredentials()->isTokenExpired()) {
    try {
        $token = $client->getOAuthACGCredentials()->refreshToken();
        // re-build the client with oauth token
        $client = $client
            ->toBuilder()
            ->oAuthACGCredentials($client->getOAuthACGCredentialsBuilder()->oAuthToken($token))
            ->build();
    } catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
        // handle exception
    }
}
```

If a token expires, an exception will be thrown before the next endpoint call requiring authentication.

### Storing an access token for reuse

It is recommended that you store the access token for reuse.

```php
// store token
$_SESSION['access_token'] = $client->getOAuthACGCredentials()->getOAuthToken();
```

### Creating a client from a stored token

To authorize a client using a stored access token, just set the access token in Configuration along with the other configuration parameters before creating the client:

```php
// load token later...
$token = $_SESSION['access_token'];

// re-build the client with oauth token
$client = $client
    ->toBuilder()
    ->oAuthACGCredentials($client->getOAuthACGCredentialsBuilder()->oAuthToken($token))
    ->build();
```

### Complete example



```php
<?php
require_once __DIR__.'/vendor/autoload.php';

session_start();

use MultiAuthSampleLib\Environment;
use MultiAuthSampleLib\Authentication\OAuthACGCredentialsBuilder;
use MultiAuthSampleLib\Models\OAuthScopeOAuthACGEnum;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->oAuthACGCredentials(
        OAuthACGCredentialsBuilder::init(
            'OAuthClientId',
            'OAuthClientSecret',
            'OAuthRedirectUri'
        )
            ->oAuthScopes(
                [
                    OAuthScopeOAuthACGEnum::READ_SCOPE
                ]
            )
    )
    ->accessToken2('accessToken')
    ->environment(Environment::TESTING)
    ->build();


// Obtain access token, restore from cache if possible
if (isset($_SESSION['access_token'])) {
    $token = $_SESSION['access_token'];
    // re-build the client with oauth token
    $client = $client
        ->toBuilder()
        ->oAuthACGCredentials($client->getOAuthACGCredentialsBuilder()->oAuthToken($token))
        ->build();
} else {
    try {
        // build authorization url to redirect the user
        $oAuthRedirectUri = $client->getOAuthACGCredentials()->buildAuthorizationUrl();
        // redirect the user to $oAuthRedirectUri and get a code after the user consent
        header('Location: ' . filter_var($oAuthRedirectUri, FILTER_SANITIZE_URL));

        // fetch an oauth token to authorize the client using the stored code
        $token = $client->getOAuthACGCredentials()->fetchToken($_GET['code']);
        // re-build the client with oauth token
        $client = $client
            ->toBuilder()
            ->oAuthACGCredentials($client->getOAuthACGCredentialsBuilder()->oAuthToken($token))
            ->build();

        // store token
        $_SESSION['access_token'] = $token;
    } catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
        // handle exception
    }
}

// check if token gets expired, then try to refresh the token
if ($client->getOAuthACGCredentials()->isTokenExpired()) {
    try {
        // refresh the token
        $token = $client->getOAuthACGCredentials()->refreshToken();
        // re-build the client with oauth token
        $client = $client
            ->toBuilder()
            ->oAuthACGCredentials($client->getOAuthACGCredentialsBuilder()->oAuthToken($token))
            ->build();

        // update the cached token
        $_SESSION['access_token'] = $token;
    } catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
        // handle exception
    }
}

// the client is now authorized; you can use $client to make endpoint calls
```


