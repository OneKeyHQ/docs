# Init SDK

## Initialization

Initialize the SDK and returns the initialization result to the caller.

```typescript
HardwareSDK.init(params): Promise&lt;void&gt;;
```

### Params

* `debug` — _optional_ `boolean` Enable debug logging for development and troubleshooting
* `fetchConfig` — _optional_ `boolean` Query device version information over network for update prompts and feature compatibility. Default: `false`
* `connectSrc` — _optional_ `string` URL of the iframe page for OneKey Bridge communication (required for `@onekeyfe/hd-web-sdk`)
* `env` — _optional_ `'web' | 'react-native' | 'lowlevel'` SDK environment type

### Version Compatibility

The `connectSrc` URL version should match your installed SDK version:
```
SDK Version    ConnectSrc URL
0.3.30         https://jssdk.onekey.so/0.3.30/
0.3.34         https://jssdk.onekey.so/0.3.34/
```

### Error Handling

Common initialization errors:
- Bridge not installed or not running
- Invalid `connectSrc` URL
- Network connectivity issues
- Unsupported environment configuration

### Example

```typescript
try {
  await HardwareSDK.init({
    debug: true, // Enable for development
    fetchConfig: false,
    connectSrc: 'https://jssdk.onekey.so/0.3.34/', // Required for web SDK
    env: 'web' // Optional, defaults based on package
  });
  console.log('SDK initialized successfully');
} catch (error) {
  console.error('SDK initialization failed:', error);
  // Handle initialization errors appropriately
}
```

### Environment-specific Notes

#### Web SDK
- Requires OneKey Bridge installation
- Uses iframe for Bridge communication
- Needs valid `connectSrc` URL

#### React Native
- Handles Bluetooth permissions automatically
- No `connectSrc` needed
- Set `env: 'react-native'`

#### Low-level
- Requires custom transport implementation
- Set `env: 'lowlevel'`
- See [low-level transport plugin](../advanced/low-level-transport-plugin.md) documentation
