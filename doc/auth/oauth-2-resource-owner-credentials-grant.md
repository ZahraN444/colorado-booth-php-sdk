
# OAuth 2 Resource Owner Credentials Grant



Documentation for accessing and setting credentials for OAuthROPCG.

## Auth Credentials

| Name | Type | Description | Setter | Getter |
|  --- | --- | --- | --- | --- |
| OAuthClientId | `string` | OAuth 2 Client ID | `oAuthClientId` | `getOAuthClientId()` |
| OAuthClientSecret | `string` | OAuth 2 Client Secret | `oAuthClientSecret` | `getOAuthClientSecret()` |
| OAuthUsername | `string` | OAuth 2 Resource Owner Username | `oAuthUsername` | `getOAuthUsername()` |
| OAuthPassword | `string` | OAuth 2 Resource Owner Password | `oAuthPassword` | `getOAuthPassword()` |
| OAuthToken | `OAuthToken\|null` | Object for storing information about the OAuth token | `oAuthToken` | `getOAuthToken()` |
| OAuthClockSkew | `int` | Clock skew time in seconds applied while checking the OAuth Token expiry. | `oAuthClockSkew` | - |
| OAuthTokenProvider | `callable(OAuthToken, OAuthROPCGManager): OAuthToken` | Registers a callback for oAuth Token Provider used for automatic token fetching/refreshing. | `oAuthTokenProvider` | - |
| OAuthOnTokenUpdate | `callable(OAuthToken): void` | Registers a callback for token update event. | `oAuthOnTokenUpdate` | - |



**Note:** Auth credentials can be set using `OAuthROPCGCredentialsBuilder::init()` in `oAuthROPCGCredentials` method in the client builder and accessed through `getOAuthROPCGCredentials` method in the client instance.

## Usage Example

### Client Initialization

You must initialize the client with *OAuth 2.0 Resource Owner Password Credentials Grant* credentials as shown in the following code snippet.

```php
use MultiAuthSampleLib\Authentication\OAuthROPCGCredentialsBuilder;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->oAuthROPCGCredentials(
        OAuthROPCGCredentialsBuilder::init(
            'OAuthClientId',
            'OAuthClientSecret',
            'OAuthUsername',
            'OAuthPassword'
        )
    )
    ->build();
```



Your application must obtain user authorization before it can execute an endpoint call in case this SDK chooses to use *OAuth 2.0 Resource Owner Password Credentials Grant*. This authorization includes the following steps

The `fetchToken()` method will exchange the user's credentials for an *access token*. The access token is an object containing information for authorizing client requests and refreshing the token itself.

```php
try {
    $token = $client->getOAuthROPCGCredentials()->fetchToken();
    // re-build the client with oauth token
    $client = $client
        ->toBuilder()
        ->oAuthROPCGCredentials($client->getOAuthROPCGCredentialsBuilder()->oAuthToken($token))
        ->build();
} catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
    // handle exception
}
```

The client can now make authorized endpoint calls.

### Refreshing the token

An access token may expire after sometime. To extend its lifetime, you must refresh the token.

```php
if ($client->getOAuthROPCGCredentials()->isTokenExpired()) {
    try {
        $token = $client->getOAuthROPCGCredentials()->refreshToken();
        // re-build the client with oauth token
        $client = $client
            ->toBuilder()
            ->oAuthROPCGCredentials($client->getOAuthROPCGCredentialsBuilder()->oAuthToken($token))
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
$_SESSION['access_token'] = $client->getOAuthROPCGCredentials()->getOAuthToken();
```

### Creating a client from a stored token

To authorize a client using a stored access token, just set the access token in Configuration along with the other configuration parameters before creating the client:

```php
// load token later...
$token = $_SESSION['access_token'];

// re-build the client with oauth token
$client = $client
    ->toBuilder()
    ->oAuthROPCGCredentials($client->getOAuthROPCGCredentialsBuilder()->oAuthToken($token))
    ->build();
```

### Complete example



```php
<?php
require_once __DIR__.'/vendor/autoload.php';

session_start();

// Client configuration
use MultiAuthSampleLib\Environment;
use MultiAuthSampleLib\Authentication\OAuthROPCGCredentialsBuilder;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->oAuthROPCGCredentials(
        OAuthROPCGCredentialsBuilder::init(
            'OAuthClientId',
            'OAuthClientSecret',
            'OAuthUsername',
            'OAuthPassword'
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
        ->oAuthROPCGCredentials($client->getOAuthROPCGCredentialsBuilder()->oAuthToken($token))
        ->build();
} else {
    try {
        // fetch an oauth token to authorize the client
        $token = $client->getOAuthROPCGCredentials()->fetchToken();
        // re-build the client with oauth token
        $client = $client
            ->toBuilder()
            ->oAuthROPCGCredentials($client->getOAuthROPCGCredentialsBuilder()->oAuthToken($token))
            ->build();

        // store token
        $_SESSION['access_token'] = $token;
    } catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
        // handle exception
    }
}

// check if token gets expired, then try to refresh the token
if ($client->getOAuthROPCGCredentials()->isTokenExpired()) {
    try {
        // refresh the token
        $token = $client->getOAuthROPCGCredentials()->refreshToken();
        // re-build the client with oauth token
        $client = $client
            ->toBuilder()
            ->oAuthROPCGCredentials($client->getOAuthROPCGCredentialsBuilder()->oAuthToken($token))
            ->build();

        // update the cached token
        $_SESSION['access_token'] = $token;
    } catch (MultiAuthSampleLib\Exceptions\ApiException $e) {
        // handle exception
    }
}

// the client is now authorized; you can use $client to make endpoint calls
```


