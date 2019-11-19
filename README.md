# serverless-token-auth

A super simple NodeJS implementation of Token Auth. Designed to store user information in either Redis, DynamoDB or Cognito

## Getting Started
Add the authorizer into your YAML file for your existing service
```YAML
  functions:
  create:
    handler: index.main
    events:
      - http:
          path: /{any+}
          method: any
          authorizer:
            arn: xxx:xxx:Lambda-TokenAuth
            resultTtlInSeconds: 0
            identitySource: method.request.header.Authorization
            identityValidationExpression: someRegex
```

### Requesting a token
```js
  //request a token
  {
    username: "bob123@gmail.com",
    password: "12345678"
  }
```

```js
  // response
  {token: "2f29b132-1c08-4709-b441-978ebc688ef5", user_id: "345", expiry: 1574136906, data: {...}}
```
| Field  | Type | Description |
| ------------- | ------------- | ------------- |
| `token`  | String |  UUID generated token  |
| `user_id`  | String | The user id provided by the server creating the user |
| `expiry`  | Integer | unix timestamp of token expiry  |
| `data`  | JSON |  Optional data you want to store on the token eg. the users name, email  |
