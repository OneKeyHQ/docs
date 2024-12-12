# Tutorial: Get started with OneKey

## Prerequisites
- Node.js version >= 14.0.0
- OneKey Bridge installed (for Web SDK)
- A OneKey hardware wallet device

### Installing OneKey Bridge (Web SDK Only)
1. Download OneKey Bridge from [official website](https://onekey.so/download/)
2. Install and verify Bridge status:
   ```bash
   # Check if Bridge is running (should return version info)
   curl http://127.0.0.1:21320/version
   ```

### Installing and Initializing the SDK

Select the type of your project, and follow the installation instructions.

#### Web

* Install `@onekeyfe/hd-web-sdk` package.

```bash
npm install --save @onekeyfe/hd-web-sdk
```

* Initialize the SDK.

**connectSrc**: The official web page deployed by OneKey is used to create an iframe on the page to communicate with OneKey Bridge.&#x20;

The complete link to the web page is [`https://jssdk.onekey.so/0.3.30/iframe.html`](https://jssdk.onekey.so/0.3.30/iframe.html).

Normally, the number after the URL should match the version number of the SDK you installed. For example, "0.3.30" in this case.

If encountering issues with the web page failing to load for the corresponding version number, please try using a different version of the SDK or submit an issue on [GitHub](https://github.com/OneKeyHQ/hardware-js-sdk/issues) for feedback. We will work quickly to resolve the problem.

```javascript
import { HardwareWebSdk as HardwareSDK } from '@onekeyfe/hd-web-sdk';

try {
  await HardwareSDK.init({
    debug: true, // Enable for development
    connectSrc: 'https://jssdk.onekey.so/0.3.30/',
    // Set to true if you need to use API related to firmware version checking
    fetchConfig: false,
  });
  console.log('SDK initialized successfully');
} catch (error) {
  console.error('Failed to initialize SDK:', error);
  // Handle initialization errors
  // Common issues:
  // - Bridge not installed/running
  // - Invalid connectSrc
  // - Network connectivity issues
}
```

* Proceed to the [next step](#subscribe-ui-event).

#### React Native

* Install `@onekeyfe/hd-ble-sdk` package.

```bash
npm install --save @onekeyfe/hd-ble-sdk
```

* Initialize the SDK.

```javascript
import HardwareSDK from '@onekeyfe/hd-ble-sdk';

try {
  await HardwareSDK.init({
    debug: true // Enable for development
  });
  console.log('SDK initialized successfully');
} catch (error) {
  console.error('Failed to initialize SDK:', error);
  // Handle initialization errors
}
```

* Proceed to the [next step](#subscribe-ui-event).

#### Node.js

* Install `@onekeyfe/hd-common-connect-sdk` package.

```bash
npm install --save @onekeyfe/hd-common-connect-sdk
```

* Initialize the SDK.

```javascript
import HardwareSDK from '@onekeyfe/hd-common-connect-sdk';

try {
  await HardwareSDK.init({
    debug: true,
    env: 'node'
  });
  console.log('SDK initialized successfully');
} catch (error) {
  console.error('Failed to initialize SDK:', error);
  // Handle initialization errors
}
```

* Proceed to the [next step](#subscribe-ui-event).

#### iOS Native / Android Native / Flutter / Other platform

* To enable communication with JavaScript, for example, iOS or Android can use methods such as WebView’s JSBridge or JSCore.
* Install `@onekey/hd-common-connect-sdk` package at the JSBridge layer.

```bash
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

* Proceed to the [next step](#subscribe-ui-event).

### Subscribe UI event <a href="#subscribe-ui-event" id="subscribe-ui-event"></a>

To improve the user experience when calling methods like getAddress and signTransaction, it is recommended to provide UI prompts for device unlocking and confirmation through event handling.

Register these common UI events before using the SDK:

```javascript
import { UI_EVENT, UI_REQUEST, UI_RESPONSE, CoreMessage } from '@onekeyfe/hd-core';

// Device state tracking
let isDeviceProcessing = false;

HardwareSDK.on(UI_EVENT, async (message: CoreMessage) => {
  try {
    switch (message.type) {
      case UI_REQUEST.REQUEST_PIN:
        // Show PIN input UI to user
        isDeviceProcessing = true;
        // Enter the PIN code on the device
        await HardwareSDK.uiResponse({
          type: UI_RESPONSE.RECEIVE_PIN,
          payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
        });
        break;

      case UI_REQUEST.REQUEST_PASSPHRASE:
        // Show passphrase input UI
        isDeviceProcessing = true;
        // Enter the passphrase on the device
        await HardwareSDK.uiResponse({
          type: UI_RESPONSE.RECEIVE_PASSPHRASE,
          payload: {
            value: '',
            passphraseOnDevice: true,
            save: false,
          },
        });
        break;

      case UI_REQUEST.REQUEST_BUTTON:
        // Show "Confirm on device" UI
        isDeviceProcessing = true;
        // Optional: Display what action needs confirmation
        break;

      case UI_REQUEST.CLOSE_UI_WINDOW:
        isDeviceProcessing = false;
        // Hide any active UI prompts
        break;
    }
  } catch (error) {
    console.error('UI event handling error:', error);
    isDeviceProcessing = false;
    // Handle UI interaction errors
  }
});

// Handle device disconnection
HardwareSDK.on('DEVICE_EVENT', (event) => {
  if (event.type === 'DEVICE_DISCONNECT') {
    isDeviceProcessing = false;
    // Handle device disconnection (e.g., show reconnect UI)
  }
});
```

### Get your Bitcoin address

After completing SDK initialization and UI event setup, let's get the first Native Segwit Bitcoin address from the hardware wallet.

First, search for available devices:

```javascript
async function getConnectedDevice() {
  try {
    const searchDeviceResponse = await HardwareSDK.searchDevices();
    if (!searchDeviceResponse.success) {
      throw new Error('Device search failed');
    }

    if (searchDeviceResponse.payload.length === 0) {
      throw new Error('No devices found');
    }

    // Use the first device found
    return searchDeviceResponse.payload[0];
  } catch (error) {
    console.error('Failed to find device:', error);
    throw error;
  }
}
```

Get the first Native Segwit Bitcoin address:

```javascript
async function getBitcoinAddress(device) {
  try {
    const params = {
      path: "m/84'/0'/0'/0/0",
      coin: 'btc',
      showOnOneKey: true, // Display address on device
    };

    const response = await HardwareSDK.btcGetAddress(
      device.connectId,
      device.deviceId,
      params
    );

    if (!response.success) {
      throw new Error(response.payload?.error || 'Failed to get address');
    }

    console.log('Your first native segwit bitcoin address:', response.payload.address);
    return response.payload.address;
  } catch (error) {
    console.error('Failed to get Bitcoin address:', error);
    throw error;
  }
}

// Usage example
async function main() {
  try {
    const device = await getConnectedDevice();
    const address = await getBitcoinAddress(device);
    // Use the address...
  } catch (error) {
    // Handle errors appropriately
    console.error('Operation failed:', error);
  }
}
```

Congratulations! You've learned the basic SDK usage pattern. All other API methods follow a similar pattern:
1. Find connected device
2. Prepare method parameters
3. Handle UI events
4. Call API method
5. Handle success/error responses

### Advanced: Passphrase

Our wallet supports hidden wallets through passphrases. Once you understand the basic API calling methods, using hidden wallets is straightforward - just add the appropriate parameter in Common Params. See the [Passphrase](advanced/passphrase.md) documentation for details.

### Troubleshooting Common Issues

1. Bridge Connection (Web SDK)
   - Verify Bridge is running: `curl http://127.0.0.1:21320/version`
   - Check Bridge logs for errors
   - Ensure correct SDK version in connectSrc

2. Device Not Found
   - Verify device is connected and unlocked
   - Check USB/Bluetooth permissions
   - Try reconnecting the device

3. API Call Failures
   - Enable debug mode during initialization
   - Check error response details
   - Verify parameter formats
   - Ensure proper event handling

For more detailed troubleshooting, refer to our [GitHub issues](https://github.com/OneKeyHQ/hardware-js-sdk/issues) or contact support.
