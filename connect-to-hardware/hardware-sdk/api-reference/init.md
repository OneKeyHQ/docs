# init

## initialization

Initialize the SDK and returns the initialization result to the caller.

```typescript
HardwareSDK.init(params);
```

### Params

* `debug` — _optional_ `boolean` print debug log option
* `connectSrc` — _optional_ `string` the `url` of the `iframe` page, when using the `@onekeyfe/hd-web-sdk` package, needs to be passed in the path of the `iframe` in order to establish the connection.
* `env` — _optional_ `'web' | 'react-native'` the environment to use the SDK

### Example

```typescript
HardwareSDK.init({
    debug: false,
    connectSrc: 'https://jssdk.onekey.so/0.2.14/', // if use web sdk
});
```
