# Initialize the SDK

Initialize the SDK before discovering devices or calling other methods. Import the entry point for your runtime; see [Install SDK](../install-sdk.md).

```typescript
await HardwareSDK.init(params);
```

## Parameters

* `debug` — optional `boolean`. Enables debug logging. Keep it disabled in production unless needed for a controlled diagnostic session.
* `fetchConfig` — optional `boolean`. Loads remote firmware configuration used by release checks.
* `connectSrc` — optional `string`. Base URL of the matching iframe deployment when using `@onekeyfe/hd-web-sdk` with OneKey Bridge. This is not a React Native BLE setting.
* `env` — optional runtime/transport selector for the selected SDK entry point. The common SDK supports environments including `node`, `lowlevel`, and `webusb`; use the value documented for your integration.

## Bridge example

```typescript
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';

// Set this to the iframe deployment matching the SDK version you installed.
const connectSrc = 'https://jssdk.onekey.so/1.0.38/';
await HardwareSDK.init({
  debug: false,
  fetchConfig: true,
  connectSrc,
});
```

The versioned URL is an example, not a recommendation to install that version. Register [UI event handlers](../config-event.md) before making calls that may require user interaction.
