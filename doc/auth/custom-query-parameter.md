
# Custom Query Parameter



Documentation for accessing and setting credentials for apiKey.

## Auth Credentials

| Name | Type | Description | Setter | Getter |
|  --- | --- | --- | --- | --- |
| token | `string` | - | `token` | `getToken()` |
| api-key | `string` | - | `apiKey` | `getApiKey()` |



**Note:** Auth credentials can be set using `ApiKeyCredentialsBuilder::init()` in `apiKeyCredentials` method in the client builder and accessed through `getApiKeyCredentials` method in the client instance.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```php
use MultiAuthSampleLib\Authentication\ApiKeyCredentialsBuilder;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->apiKeyCredentials(
        ApiKeyCredentialsBuilder::init(
            'token',
            'api-key'
        )
    )
    ->build();
```


