# binanceGetAddress

import Playground from '@src/components/playground';

### Binance: get address

Display requested address derived by given BIP44 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

ES6

```javascript
const result = await OneKeyConnect.binanceGetAddress(params);
```

CommonJS

```javascript
OneKeyConnect.binanceGetAddress(params).then(function (result) {

});
```

#### Params

**Optional common params**

**Exporting single address**

* `path` - _required_ `string | Array<number>` minimum length is `5`. read more
* `address` - _optional_ `string` address for validation (read `Handle button request` section below)
* `showOnOneKey` - _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

**Handle button request**

There is a possibility to handle `UI.ADDRESS_VALIDATION` event which will be triggered once the address is displayed on the device. You can handle this event and display custom UI inside of your application.

If certain conditions are fulfilled popup will not be used at all:

* the user gave permissions to communicate with OneKey
* device is authenticated by pin/passphrase
* application has `OneKeyConnect.on(UI.ADDRESS_VALIDATION, () => {});` listener registered
* parameter `address` is set
* parameter `showOnOneKey` is set to `true` (or not set at all)
* application is requesting ONLY ONE address

#### Example

Display address of first Binance account:

```javascript
OneKeyConnect.binanceGetAddress({
  path: "m/44'/714'/0'/0/0",
});
```

Return a bundle of Binance addresses without displaying them on device:

```javascript
OneKeyConnect.binanceGetAddress({
  bundle: [
    { path: "m/44'/714'/0'/0/0", showOnOneKey: false }, // account 1, address 1
    { path: "m/44'/714'/1'/0/1", showOnOneKey: false }, // account 2, address 2
    { path: "m/44'/714'/2'/0/2", showOnOneKey: false }, // account 3, address 3
  ],
});
```

Validate address using custom UI inside of your application:

```javascript
import OneKeyConnect, { UI } from '@onekeyfe/connect';

OneKeyConnect.on(UI.ADDRESS_VALIDATION, (data) => {
  console.log('Handle button request', data.address, data.serializedPath);
  // here you can display custom UI inside of your app
});

const result = await OneKeyConnect.binanceGetAddress({
  path: "m/44'/714'/0'/0/0",
  address: 'bnb1afwh46v6nn30nkmugw5swdmsyjmlxslgjfugre',
});
// don't forget to hide your custom UI after you get the result!
```

#### Result

Result with only one address

```javascript
{
    success: true,
    payload: {
        path: Array<number>, // hardended path
        serializedPath: string,
        address: string,
    }
}
```

Result with bundle of addresses

```javascript
{
    success: true,
    payload: [
        { path: Array<number>, serializedPath: string, address: string }, // account 1, address 1
        { path: Array<number>, serializedPath: string, address: string }, // account 2, address 2
        { path: Array<number>, serializedPath: string, address: string }  // account 3, address 3
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

\<Playground initValue={`OneKeyConnect.binanceGetAddress({ path: "m/44'/714'/0'/0/0" });`} />
