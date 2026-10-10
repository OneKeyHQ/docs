# Getting Started

Use the Hardware SDK to discover a OneKey device, request an address, or ask the user to sign a transaction.

## 1. Choose and initialize an SDK

Follow [Install the SDK](install-sdk.md) for your runtime. The bridge-based web integration requires [OneKey Bridge](https://onekey.so/download?client=bridge). Native Bluetooth integrations use a different transport.

For a current browser integration with WebUSB, see the [developer portal](https://developer.onekey.so/en/hardware-sdk/getting-started). Do not assume that every browser transport requires Bridge.

The [online debugging tool](https://hardware-example.onekeytest.com/) can help explore the available APIs. Choose the transport supported by your device and browser.

## 2. Register UI event handlers

Before making device calls, handle PIN, passphrase, and confirmation events. See [Handle SDK Events](config-event.md). The application must respond once to each input request or cancel the pending operation.

## 3. Discover and identify the device

1. Call [searchDevices](api-reference/basic-api/search-devices.md).
2. Let the user select the intended device and retain its `connectId` for the current connection.
3. If discovery does not provide `deviceId`, call [getFeatures](api-reference/basic-api/get-features.md) and read `payload.device_id` after checking success.

Bluetooth discovery does not initialize or pair every discovered device, so it can return a null `deviceId`. See [Common Parameters](api-reference/common-params.md) for the distinction between routing and identity.

## 4. Call an API and handle its response

Most chain methods return a promise with a discriminated success/error result:

```typescript
export interface Unsuccessful {
  success: false;
  payload: {
    error: string;
    code?: string | number;
  };
}

export interface Success<T> {
  success: true;
  payload: T;
}

export type Response<T> = Promise<Success<T> | Unsuccessful>;
```

For example, after obtaining `connectId` and `deviceId`:

```typescript
const response = await HardwareSDK.btcGetAddress(connectId, deviceId, {
  path: "m/84'/0'/0'/0/0",
  coin: 'btc',
  showOnOneKey: true,
  useEmptyPassphrase: true,
});

if (response.success === false) {
  throw new Error(response.payload.error);
}
const address = response.payload.address;
```

`useEmptyPassphrase: true` selects the standard wallet. To support hidden wallets, follow the [Passphrase guide](advanced/passphrase.md).

See the [API reference](api-reference/), [derivation paths](api-reference/path.md), and [error codes](api-reference/error-code.md). Handle cancellation as a normal user action; do not automatically retry signing or device-management operations.

## 5. Build the integration

Follow the [address tutorial](tutorial.md) for an end-to-end example. For native platforms that provide their own transport, see the [Common SDK guide](advanced/common-sdk-guide.md).

## Develop the SDK locally

Use the setup instructions in the [hardware-js-sdk repository](https://github.com/OneKeyHQ/hardware-js-sdk). Build commands and prerequisites depend on the checked-out version.
