# Low-level transport plugin

Our Bluetooth SDK is developed using React Native. You may not be using React Native to develop your application, for example, you may be using Swift, Kotlin, or Flutter.At this point, adding a dependency on React Native to your project may not be a good choice.

We provide a new communication method where you can use the LowlevelTransportPlugin to communicate with the hardware. You are free to use any technology stack of your choice. You will need to finish the protocol for device connect, disconnect, send data, and receive data.

Since the Bluetooth data sent by the device is 64 bytes per packet, the receive method needs to externally determine whether a complete data packet has been received. You must return the complete data packet, otherwise the device will not be able to correctly parse the data.

Refer to the [Message protocol](onekey-message-protocol.md).

### LowlevelTransportSharedPlugin

```typescript
export type LowLevelDevice = { id: string; name: string };
export type LowlevelTransportSharedPlugin = {
  enumerate: () => Promise<LowLevelDevice[]>;
  send: (uuid: string, data: string) => Promise<void>;
  receive: () => Promise<string>;
  connect: (uuid: string) => Promise<void>;
  disconnect: (uuid: string) => Promise<void>;

  init: () => Promise<void>;
  version: string;
};
```

### Example

Here is an [iOS demo](https://github.com/originalix/Hardware-Lowlevel-Communicate) written in Swift. Bridging native side and SDK through a WebView. Handling connections and data transmission using CoreBluetooth.

```typescript
import HardwareSDK from '@onekeyfe/hd-common-connect-sdk'

function createLowlevelPlugin() {
  const plugin = {
    enumerate: () => {
      return Promise.resolve([{id: 'foo', name: 'bar'}])
    },
    send: (uuid, data) => {
      // TODO: send data
    },
    receive: () => {
      return Promise.resolve(result hex string)
    },
    connect: (uuid) => {
      // TODO: connect device
    },
    disconnect: (uuid)  => {
      // TODO: disconnect device
    },

    init: () => {
      // TODO: do some init work
    },

    version: 'OneKey-1.0'
  }
  return plugin
}

const plugin = createLowlevelPlugin()

const settings = {
  env: 'lowlevel', // LowlevelTransport requires the use of a lowlevel environment to be enabled.
  debug: true 
}

HardwareSDK.init(settings, undefined, plugin)
```
