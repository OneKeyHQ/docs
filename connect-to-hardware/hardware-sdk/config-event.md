# Handle SDK Events

Register event listeners before calling methods that may ask the user to unlock or confirm on the device.

```typescript
HardwareSDK.on(event, callback);
```

`event` selects a category, such as `UI_EVENT`. The callback receives a message with `event`, `type`, and `payload` fields. Use `type` to select the handler.

## Respond to UI requests

A PIN or passphrase request pauses the pending operation until your application responds or cancels it. Send **one response per request**. The example below chooses device entry; offer that option only when supported by the connected device.

```typescript
import { UI_EVENT, UI_REQUEST, UI_RESPONSE } from '@onekeyfe/hd-core';

HardwareSDK.on(UI_EVENT, (message) => {
  if (message.type === UI_REQUEST.REQUEST_PIN) {
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PIN,
      payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
    });
  } else if (message.type === UI_REQUEST.REQUEST_PASSPHRASE) {
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PASSPHRASE,
      payload: {
        value: '',
        passphraseOnDevice: true,
        save: false,
      },
    });
  } else if (message.type === UI_REQUEST.REQUEST_BUTTON) {
    // Show a prompt asking the user to review the device screen.
  } else if (message.type === UI_REQUEST.CLOSE_UI_WINDOW) {
    // Close the prompt for the completed or cancelled interaction.
  }
});
```

For software entry, send the PIN matrix positions or passphrase through [uiResponse](api-reference/basic-api/response-ui-event.md). Do not submit both software and device-entry responses to the same event. Use [cancel](api-reference/basic-api/cancel-request.md) when the user cancels.

Current SDK versions may attach `interactionId` and `deviceId` to UI requests. Preserve and return these correlation fields when provided, particularly when more than one device can be active. See the [current UI response reference](https://developer.onekey.so/en/hardware-sdk/basic-api/response-ui-event) for the version-specific contract.

## Event categories

* `UI_EVENT`: PIN, passphrase, device confirmation, permission, progress, and window-closing requests.
* `DEVICE_EVENT`: device information and capability updates.
* `FIRMWARE_EVENT`: firmware release information.

`UI_RESPONSE` contains response constants; it is not an event category to subscribe to. Import the constants from `@onekeyfe/hd-core` instead of duplicating string values.
