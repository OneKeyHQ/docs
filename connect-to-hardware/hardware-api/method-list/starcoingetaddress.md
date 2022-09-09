# starcoinGetAddress

import Playground from "@src/components/playground";

### Starcoin: Get address

Display requested address derived by given path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

:::warning warning

* This method is supported after OneKey Connect JS-SDK version v8.6.0, and npm modules equal or larger than this version can be used.
* The OneKey device firmware may also be different for the user, caller should handle the response of method like firmware not support issue. :::

ES6

```javascript
const result = await OneKeyConnect.starcoinGetAddress(params);
```

CommonJS

```javascript
OneKeyConnect.starcoinGetAddress(params).then(function(result) {

});
```

#### Params

**Optional common params**

**Exporting single address**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `address` — _optional_ `string` address for validation (read `Handle button request` section below)
* `showOnDevice` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnDevice` fields

**Handle button request**

There is a possibility to handle `UI.ADDRESS_VALIDATION` event which will be triggered once the address is displayed on the device. You can handle this event and display custom UI inside of your application.

If certain conditions are fulfilled popup will not be used at all:

* the user gave permissions to communicate with OneKey
* device is authenticated by pin/passphrase
* application has `OneKeyConnect.on(UI.ADDRESS_VALIDATION, () => {});` listener registered
* parameter `address` is set
* parameter `showOnDevice` is set to `true` (or not set at all)
* application is requesting ONLY ONE address

#### Example

Display address of first starcoin account:

```javascript
OneKeyConnect.starcoinGetAddress({
    path: "m/44'/101010'/0'/0'/0'"
});
```

Return a bundle of starcoin addresses without displaying them on device:

```javascript
OneKeyConnect.starcoinGetAddress({
    bundle: [
        { path: "m/44'/101010'/0'/0'/0'", showOnDevice: false }, // account 1
        { path: "m/44'/101010'/0'/0'/1'", showOnDevice: false }, // account 2
        { path: "m/44'/101010'/0'/0'/2'", showOnDevice: false }  // account 3
    ]
});
```

Validate address using custom UI inside of your application:

```javascript
import OneKeyConnect, { UI } from '@onekeyfe/connect';

OneKeyConnect.on(UI.ADDRESS_VALIDATION, data => {
    console.log("Handle button request", data.address, data.serializedPath);
    // here you can display custom UI inside of your app
});

const result = await OneKeyConnect.starcoinGetAddress({
    path: "m/44'/101010'/0'/0'/0'",
});
// dont forget to hide your custom UI after you get the result!
```

#### Result

Result with only one address

```javascript
{
    success: true,
    payload: {
        address: string,     // displayed address
        path: Array<number>, // hardended path
        serializedPath: string,
    }
}
```

Result with bundle of addresses sorted by FIFO

```javascript
{
    success: true,
    payload: [
        { address: string, path: Array<number>, serializedPath: string }, // account 1
        { address: string, path: Array<number>, serializedPath: string }, // account 2
        { address: string, path: Array<number>, serializedPath: string }  // account 3
    ]
}
```

Error

```javascript
{
    success: false,
    payload: {
        error: string // error message
    }
}
```

#### Error Detail

1. firmware not support, must upgrade to the latest version.

```json
{
  "success": false,
  "payload": {
    "error": "Unknown message",
    "code": "Failure_UnexpectedMessage"
  }
}
```

\<Playground initValue={ `OneKeyConnect.starcoinGetAddress({ path: "m/44'/101010'/0'/0'/0'" });`} />
