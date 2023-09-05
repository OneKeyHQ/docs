# Event

Once the user has completed initialization, HardwareSDK sends out events regarding device information, UI requests, device requests, etc.

Event objects are usually of the following types.

```typescript
{
  event: string;
  type: string;
  payload: any;
}
```

The `event` field is a classification of events, you can distinguish specific events by the `type` field of the event object, you also import all types of constants in the `@onekeyfe/hd-core` library.

## Subscribe to the event

```typescript
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';
import { UI_EVENT, UI_RESPONSE, CoreMessage } from '@onekeyfe/hd-core';

HardwareSDK.on(UI_EVENT, (message: CoreMessage) => {
  // Handle the PIN code input event
  if (message.type === UI_REQUEST.REQUEST_PIN) {
    // Enter the PIN code on the device
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PIN,
      payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
    });
  }
  
  // Handle the passphrase event
  if (message.type === UI_REQUEST.REQUEST_PASSPHRASE) {
    // Enter the passphrase on the device
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PASSPHRASE,
      payload: {
        value: '',
        passphraseOnDevice: true,
        save: true,
      },
    });
  }
  
  if (message.type === UI_REQUEST.REQUEST_BUTTON) {
    // Confirmation is required on the device, a UI prompt can be displayed
  }
});
```

## List of events that support subscription

* `UI_EVENT`
  * `ui-request_pin` Enter the PIN code.
  * `ui-receive_pin`
  * `ui-request_passphrase` Enter the passphrase.
  * `ui-request_passphrase_on_device` Enter the passphrase on the device.
  * `ui-button` Confirm on the device.
  * `ui-close_window` The method invocation is completed. You may close all UI prompts.
  * `ui-bluetooth_permission`
  * `ui-location_permission`
  * `ui-firmware-progress`
  * `ui-device_not_in_bootloader_mode`
* `UI_RESPONSE`
  * `ui-receive_pin` Return the PIN code value
  * `ui-receive_passphrase` Return the passphrase value
* `DEVICE_EVENT`
  * `button`
  * `pin`
  * `support_features`
  * `features`
* `FIRMWARE_EVENT`
  * `firmware-release-info`
  * `ble-firmware-release-info`
