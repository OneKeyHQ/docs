# Tutorial: Get started with OneKey

### Installing and Initializing the SDK

Select the type of your project, and follow the installation instructions.

#### Web

* Install `@onekeyfe/hd-web-sdk` package.

```
npm install --save @onekeyfe/hd-web-sdk
```

* Initialize the SDK.

```javascript
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';

HardwareSDK.init({
  debug: true,
  // The official iframe page deployed by OneKey
  connectSrc: 'https://jssdk.onekey.so/${current sdk version}/',
  // Set to true if you need to use API related to firmware version checking, 
  // otherwise keep as false.
  fetchConfig: false,
})
```

* Proceed to the [next step](tutorial-get-started-with-onekey.md#subscribe-ui-event).

#### React Native

* Install `@onekeyfe/hd-ble-sdk` package.

```
npm install --save @onekeyfe/hd-ble-sdk
```

* Initialize the SDK.

```javascript
import HardwareSDK from '@onekeyfe/hd-ble-sdk';

HardwareSDK.init({
  debug: true
})
```

* Proceed to the [next step](tutorial-get-started-with-onekey.md#subscribe-ui-event).

#### Node.js

* Install `@onekeyfe/hd-common-connect-sdk` package.

```
npm install --save @onekeyfe/hd-common-connect-sdk
```

* Initialize the SDK.

```javascript
import HardwareSDK from '@onekeyfe/hd-common-connect-sdk';

HardwareSDK.init({
  debug: true,
	env: 'node'
})
```

* Proceed to the [next step](tutorial-get-started-with-onekey.md#subscribe-ui-event).

#### iOS Native / Android Native / Flutter / Other platform

* To enable communication with JavaScript, for example, iOS or Android can use methods such as WebView’s JSBridge or JSCore.
* Install `@onekey/hd-common-connect-sdk` package at the JSBridge layer.

```
npm install --save @onekeyfe/hd-common-connect-sdk
```

*   Implement the low-level transport plugin to communicate with the native platform via JSBridge. Utilize native platform capabilities to manage device connections and send/receive data.&#x20;

    For example, the code snippet below registers some events to communicate with native via [WebViewJavascriptBridge](https://github.com/marcuswestin/WebViewJavascriptBridge) library.&#x20;

For more complete code, please refer to [Hardware-Lowlevel-Communicate repo.](https://github.com/originalix/Hardware-Lowlevel-Communicate)

```javascript
import HardwareSDK from '@onekeyfe/hd-common-connect-sdk'
import { createDeferred, isHeaderChunk, COMMON_HEADER_SIZE } from './utils'

function createLowlevelPlugin() {
  const plugin = {
    enumerate: () => {
      return new Promise((resolve) => {
        // Invoke the ‘enumerate’ method registered on the native side to retrieve the result of the device search.
        bridge.callHandler('enumerate', {}, (response) => {
	  resolve(response)
	})
      })
    },
    send: (uuid, data) => {
      return new Promise((resolve) => {
        // Invoke the ‘send’ method registered on the native side to send data to the device
        bridge.callHandler('send', {uuid, data}, (response) => {
	  resolve(response)
	})
      })
    },
    receive: () => {
      return new Promise((resolve) => {
        // Receive data from the device, 
        // verify and concatenate the data packets,
        // and only return the complete packets to the SDK.
        // You can refer to OneKey Message Protocol for this.
        // For more implementation details, please refer to the code 
        // https://github.com/originalix/Hardware-Lowlevel-Communicate/blob/main/web/index.js#L171
        runPromise = createDeferred()
        const response = runPromise.promise
        resolve(response)
      })
    },
    connect: (uuid) => {
      return new Promise((resolve) => {
        // Call the native connect method and pass in the UUID of the Bluetooth device.
        bridge.callHandler('connect', {uuid})
          // Since the connect method in the native end may be asynchronous, 
          // we also register a ‘connectFinished’ event to wait for the result returned by the native end
          bridge.registerHandler('connectFinished', () => {
             resolve()
          })
       })
    },
    disconnect: (uuid)  => {
      return new Promise((resolve) => {
        // Disconnect the device.
        bridge.callHandler('disconnect', {uuid}, (response) => {
          resolve(response)
        })
      })
    },
    init: () => {
      // Do something you want to handle during SDK initialization.
      return Promise.resolve()
    },
    version: 'OneKey-1.0'
  }
  return plugin
}

// This plugin object will be passed as a parameter during SDK initialization.
const plugin = createLowlevelPlugin()
```

* Initialize the SDK.

```javascript
import HardwareSDK from '@onekeyfe/hd-common-connect-sdk';

const settings = {
  env: 'lowlevel',
  debug: true
}

/**
 * Pass the previously written low-level plugin as the third parameter.
 */
HardwareSDK.init(settings, undefined, plugin)
```

* Proceed to the [next step](tutorial-get-started-with-onekey.md#subscribe-ui-event).

### Subscribe UI event <a href="#subscribe-ui-event" id="subscribe-ui-event"></a>

To improve the user experience when calling some methods such as getAddress and signTransaction, it is recommended to provide the user with some prompts at the UI layer for device unlocking and confirmation. This can be achieved through event passing.

It is recommended to register some common UI events before using the SDK.

```javascript
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
        save: false,
      },
    });
  }
  
  if (message.type === UI_REQUEST.REQUEST_BUTTON) {
    // Confirmation is required on the device, a UI prompt can be displayed
  }
});
```

### Get your Bitcoin address

After completing the steps of initializing the SDK and subscribe UI event listeners, we will now try to get the first Native Segwit Bitcoin address from the hardware wallet.

First, we need to search for the device and get its information.

```javascript
const searchDeviceResponse = await HardwareSDK.searchDevices()
let device
if (searchDeviceResponse.success && searchDeviceResponse.payload.length > 0) {
  // Use the first device in the search results
  device = searchDeviceResponse.payload[0]
}
```

Next, we will get the first Native Segwit Bitcoin address.&#x20;

During the execution of this method, the UI\_REQUEST.REQUEST\_PIN and UI\_REQUEST.REQUEST\_BUTTON events will be notified.

```javascript
const params = {
  path: "m/84'/0'/0'/0/0"
  coin: 'btc',
  showOnOneKey: true,
}

const response = await HardwareSDK.btcGetAddress(device.connectId, device.deviceId, params)

if (response.success) {
  console.log('Your first native segwit bitcoin address: ', response.payload.address)
}
```

Now your Bitcoin address will be printed on the console.

Congratulations! You have mastered the usage of the SDK, as the calling method for all APIs is the same.

### Advanced: Passphrase

Our wallet also supports hidden wallets. Once you have learned the basic API calling methods, it is easy for you to use the hidden wallet. You just need to add a parameter in Common Params. Please refer to the documentation related to [Passphrase](passphrase.md) for more information.
