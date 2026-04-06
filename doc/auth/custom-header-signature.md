
# Custom Header Signature



Documentation for accessing and setting credentials for apiHeader.

## Auth Credentials

| Name | Type | Description | Setter | Getter |
|  --- | --- | --- | --- | --- |
| token | `string` | - | `token` | `getToken()` |
| api-key | `string` | - | `apiKey` | `getApiKey()` |



**Note:** Auth credentials can be set using `ApiHeaderCredentialsBuilder::init()` in `apiHeaderCredentials` method in the client builder and accessed through `getApiHeaderCredentials` method in the client instance.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```php
use MultiAuthSampleLib\Authentication\ApiHeaderCredentialsBuilder;
use MultiAuthSampleLib\MultiAuthSampleClientBuilder;

$client = MultiAuthSampleClientBuilder::init()
    ->apiHeaderCredentials(
        ApiHeaderCredentialsBuilder::init(
            'token',
            'api-key'
        )
    )
    ->build();
```


