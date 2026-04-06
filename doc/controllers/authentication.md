# Authentication

```php
$authenticationController = $client->getAuthenticationController();
```

## Class Name

`AuthenticationController`

## Methods

* [Custom Authentication](../../doc/controllers/authentication.md#custom-authentication)
* [O Auth Bearer Token](../../doc/controllers/authentication.md#o-auth-bearer-token)
* [O Auth Client Credentials Grant](../../doc/controllers/authentication.md#o-auth-client-credentials-grant)
* [O Auth Authorization Grant](../../doc/controllers/authentication.md#o-auth-authorization-grant)
* [Custom Query or Header Authentication](../../doc/controllers/authentication.md#custom-query-or-header-authentication)
* [Basic Auth and Api Header Auth](../../doc/controllers/authentication.md#basic-auth-and-api-header-auth)
* [O Auth Grant Types or Combinations](../../doc/controllers/authentication.md#o-auth-grant-types-or-combinations)
* [Multiple Auth Combination](../../doc/controllers/authentication.md#multiple-auth-combination)
* [No Auth](../../doc/controllers/authentication.md#no-auth)


# Custom Authentication

```php
function customAuthentication(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->customAuthentication();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# O Auth Bearer Token

```php
function oAuthBearerToken(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->oAuthBearerToken();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# O Auth Client Credentials Grant

```php
function oAuthClientCredentialsGrant(): ServiceStatus
```

## Response Type

[`ServiceStatus`](../../doc/models/service-status.md)

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->oAuthClientCredentialsGrant();
    echo 'ServiceStatus:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# O Auth Authorization Grant

```php
function oAuthAuthorizationGrant(): User
```

## Response Type

[`User`](../../doc/models/user.md)

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->oAuthAuthorizationGrant();
    echo 'User:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# Custom Query or Header Authentication

```php
function customQueryOrHeaderAuthentication(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->customQueryOrHeaderAuthentication();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# Basic Auth and Api Header Auth

```php
function basicAuthAndApiHeaderAuth(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->basicAuthAndApiHeaderAuth();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# O Auth Grant Types or Combinations

This endpoint tests or combinations of OAuth types

```php
function oAuthGrantTypesORCombinations(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->oAuthGrantTypesORCombinations();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# Multiple Auth Combination

This endpoint uses globally applied auth which is a hypothetical scneraio but covers the worst case.

Swagger URL Endpoint 1: [http://swagger.io/endpoint1](http://swagger.io/endpoint1)

```php
function multipleAuthCombination(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->multipleAuthCombination();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```


# No Auth

**This endpoint is deprecated since version 0.0.1-alpha. You should not use this method as it requires no auth and can bring security issues to the server and api call itself!!**

This endpoint does not use auth.

Swagger URL Endpoint 1: [http://swagger.io/endpoint1](http://swagger.io/endpoint1)

:information_source: **Note** This endpoint does not require authentication.

```php
function noAuth(): string
```

## Response Type

`string`

## Example Usage

```php
$authenticationController = $client->getAuthenticationController();

try {
    $result = $authenticationController->noAuth();
    echo 'string:';
    var_dump($result);
} catch (ApiException $exp) {
    echo 'Caught:', $exp;
}
```

