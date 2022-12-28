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
import HardwareSDK, { DEVICE_EVENT, DEVICE } from '@onekeyfe/hd-web-sdk';
// or
import HardwareSDK, { DEVICE_EVENT, DEVICE } from '@onekeyfe/hd-ble-sdk';

HardwareSDK.on(DEVICE_EVENT, event => {
  if (event.type === DEVICE.PIN) {
    // Handle message with the input pin code
  }
});
```

## List of events that support subscription

* `DEVICE_EVENT`
  * `button`
  * `pin`
  * `support_features`
  * `features`
* `FIRMWARE_EVENT`
  * `firmware-release-info`
  * `ble-firmware-release-info`
* `UI_EVENT`
  * `ui-request_pin`
  * `ui-receive_pin`
  * `ui-button`
  * `ui-close_window`
  * `ui-bluetooth_permission`
  * `ui-location_permission`
  * `ui-firmware-progress`
  * `ui-device_not_in_bootloader_mode`
