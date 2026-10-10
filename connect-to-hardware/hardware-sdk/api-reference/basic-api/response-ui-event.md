# uiResponse

Responds to a pending SDK UI request. This method does not take `connectId`, `deviceId`, or common parameters as positional arguments.

```typescript
import { UI_RESPONSE } from '@onekeyfe/hd-core';

HardwareSDK.uiResponse({ type, payload });
```

## PIN input

For supported device entry:

```typescript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PIN,
  payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
});
```

For software PIN entry, `payload` is the string of keypad positions selected by the user, according to the matrix displayed on the device. Do not collect or log the literal PIN.

## Passphrase input

```typescript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PASSPHRASE,
  payload: {
    value: userEnteredPassphrase,
    passphraseOnDevice: false,
    save: false,
  },
});
```

`userEnteredPassphrase` is provided by your application's input UI. For supported on-device entry, use an empty `value` and set `passphraseOnDevice: true`.

Respond once to each request. In SDK versions that supply `interactionId` and `deviceId` correlation fields, echo those fields in the response. See [Handle SDK Events](../../config-event.md) and [Passphrase](../../advanced/passphrase.md).
