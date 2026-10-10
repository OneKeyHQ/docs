# Get Passphrase State

## Get the passphraseState of the device

Requests the selected passphrase-wallet state. The device may ask the user to enter a passphrase. Pass the successful string result as `passphraseState` on later calls to select the same wallet. A result may be undefined when no passphrase-wallet state is available.

Current SDK versions deprecate this API in favor of [openWalletSession](https://developer.onekey.so/en/hardware-sdk/basic-api/open-wallet-session). See the [Passphrase guide](../../advanced/passphrase.md) before continuing a hidden-wallet operation.

```typescript
const response = await HardwareSDK.getPassphraseState(connectId);
```

### Params

[**Optional common params**](../common-params.md)

### Example

```typescript
const response = await HardwareSDK.getPassphraseState(connectId);
```

Result

```
{
    success: true,
    payload: string | undefined
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code?: string | number // error code
    }
}
```
