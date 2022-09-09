# Handling Events

Once user grants permission for hosting page to communicate with API OneKeyConnect will emits events about device state. Events can be distinguished by "type" field of event object. Constants of all types can be imported from package.

**ES6**

```javascript
import OneKeyConnect, { DEVICE_EVENT, DEVICE } from '@onekeyfe/connect';

OneKeyConnect.on(DEVICE_EVENT, (event) => {
    if (event.type === DEVICE.CONNECT) {

    } else if (event.type === DEVICE.DISCONNECT) {

    }
});
```

**CommonJS**

```javascript
var OneKeyConnect = require('@onekeyfe/connect').default;
var DEVICE_EVENT = require('@onekeyfe/connect').DEVICE_EVENT;
var DEVICE = require('@onekeyfe/connect').DEVICE;

OneKeyConnect.on(DEVICE_EVENT, (event) => {
    if (event.type === DEVICE.CONNECT) {

    } else if (event.type === DEVICE.DISCONNECT) {

    }
});
```

## List of published events

`device-connect` `device-disconnect` `device-changed`
