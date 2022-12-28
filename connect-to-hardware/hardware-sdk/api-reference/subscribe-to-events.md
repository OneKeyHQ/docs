# Subscribe to Events

## Events

Subscribe to the SDK's events.

```typescript
HardwareSDK.on(event, callback);
```

### Params

* `event` - _required `string`_ event type, refer to [Event](broken-reference)
* `callback` - _required_ `function` callback functions

### Example

```typescript
import HardwareSDK from '@onekeyfe/hd-web-sdk';
import { DEVICE_EVENT, DEVICE } from '@onekeyfe/hd-core';

HardwareSDK.on(DEVICE_EVENT, event => {
  if (event.type === DEVICE.PIN) {
    // Handle message with the input pin code
  }
});
```
