# Init SDK

## initialization

Initialize the SDK and returns the initialization result to the caller.

```typescript
HardwareSDK.init(params);
```

### Params

* `debug` — _optional_ `boolean` print debug log option
* `fetchConfig`— _optional_ `boolean` Allows querying for updated device version information over the network, used for prompting device updates and informing which version is needed for older hardware to use new features.
* `connectSrc` — _optional_ `string` the `url` of the `iframe` page, when using the `@onekeyfe/hd-web-sdk` package, needs to be passed in the path of the `iframe` in order to establish the connection.
* `env` — _optional_ `'web' | 'react-native' | 'lowlevel'` the environment to use the SDK

### Example

```typescript
HardwareSDK.init({
    debug: false,
    fetchConfig: ,
    connectSrc: 'https://jssdk.onekey.so/0.3.34/', // if use web sdk
});
```
