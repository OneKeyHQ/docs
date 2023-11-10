# Config Event

Once the user has completed initialization, HardwareSDK sends out events regarding device information, UI requests, device requests, etc.

## Events

Subscribe to the SDK's events.

```typescript
HardwareSDK.on(event, callback);
```

### Params

* `event` - _required `string`_ event type, refer to [Event](config-event.md#list-of-events-that-support-subscription)
* `callback` - _required_ `function` callback functions

### Result

Event objects are usually of the following types.

```
{
  event: string;
  type: string;
  payload: any;
}
```

The `event` field is a classification of events, you can distinguish specific events by the `type` field of the event object, you also import all types of constants in the `@onekeyfe/hd-core` library.

## uiResponse

Some event hardware requires a feedback result. Need to use the [`uiResponse Api`](api-reference/basic-api/response-ui-event.md) to tell the hardware to process the result.

e.g Request pin, Request passphrase

```
HardwareSDK.uiResponse({
    type: string,
    payload: any
});
```

## Subscribe to the event

```typescript
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';
import { UI_EVENT, UI_RESPONSE, CoreMessage } from '@onekeyfe/hd-core';

HardwareSDK.on(UI_EVENT, (message: CoreMessage) => {
  // Handle the PIN code input event
  if (message.type === UI_REQUEST.REQUEST_PIN) {
    // Demo1 Enter the PIN code on the device
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PIN,
      payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
    });
    
    // Demo2 Software processing Pin code
    // Pseudocode
    showUIprompts(confirm: (pin)=>{
      // Tell the hardware ui request processing result
      HardwareSDK.uiResponse({
        type: UI_RESPONSE.RECEIVE_PIN,
        payload: pin,
      });
    })
  }
  
  // Handle the passphrase event
  if (message.type === UI_REQUEST.REQUEST_PASSPHRASE) {
    // Demo1 Enter the passphrase on the device
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PASSPHRASE,
      payload: {
        value: '',
        passphraseOnDevice: true,
      },
    });
    
    // Demo2 Software processing passphrase
    // Pseudocode
    showUIprompts(confirm: (passphrase)=>{
      // Tell the hardware ui request processing result
      HardwareSDK.uiResponse({
        type: UI_RESPONSE.RECEIVE_PASSPHRASE,
        payload: {
          value: passphrase,
        },
      });
    })
  }
  
  if (message.type === UI_REQUEST.REQUEST_BUTTON) {
    // Confirmation is required on the device, a UI prompt can be displayed
  }
  if (message.type === UI_REQUEST.CLOSE_UI_WINDOW) {
    // The method invocation is completed. You may close all UI prompts.
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
