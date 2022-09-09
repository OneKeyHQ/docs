# cardanoGetAddress

import Playground from "@src/components/playground";

### Cardano: get address

Display requested address derived by given [BIP32-Ed25519](https://cardanolaunch.com/assets/Ed25519\_BIP.pdf) path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

ES6

```javascript
const result = await OneKeyConnect.cardanoGetAddress(params);
```

CommonJS

```javascript
OneKeyConnect.cardanoGetAddress(params).then(function(result) {

});
```

#### Params

\*\* Optional common params\*\*

**Exporting single address**

* `addressParameters` — _required_ see description below
* `address` — _optional_ `string` address for validation (read `Handle button request` section below)
* `protocolMagic` - _required_ `Integer` 764824073 for Mainnet, 42 for Testnet
* `networkId` - _required_ `Integer` 1 for Mainnet, 0 for Testnet
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with single address fields

**Address Parameters**

[**flowtype**](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/networks/cardano.js#L37-L43)

* `addressType` - _required_ `CardanoAddressType`/`number` - you can use the flow `CARDANO.ADDRESS_TYPE` object or typescript `CardanoAddressType` enum. Supports Base, Pointer, Enterprise, Byron and Reward address types.
* `path` — _required_ `string | Array<number>` minimum length is `5`. read more
* `stakingPath` — _optional_ `string | Array<number>` minimum length is `5`. read more Used for base address derivation
* `stakingKeyHash` - _optional_ `string` hex string of staking key hash. Used for base address derivation (as an alternative to `stakingPath`)
* `certificatePointer` - _optional_ `CardanoCertificatePointer` object. Must contain `number`s `blockIndex`, `txIndex` and `certificateIndex`. ([flowtype](https://github.com/OneKeyHQ/connect/blob/onekey/src/js/types/networks/cardano.js#L31-L35)) Used for pointer address derivation. [read more about pointer address](https://hydra.iohk.io/build/2006688/download/1/delegation\_design\_spec.pdf#subsubsection.3.2.2)

**Handle button request**

Since `onekey-connect@8.0.0` there is a possibility to handle `UI.ADDRESS_VALIDATION` event which will be triggered once the address is displayed on the device. You can handle this event and display custom UI inside of your application.

If certain conditions are fulfilled popup will not be used at all:

* the user gave permissions to communicate with OneKey
* device is authenticated by pin/passphrase
* application has `OneKeyConnect.on(UI.ADDRESS_VALIDATION, () => {});` listener registered
* parameter `address` is set
* parameter `showOnOneKey` is set to `true` (or not set at all)
* application is requesting ONLY ONE(!) address

#### Example

Display byron address of first cardano account:

```javascript
OneKeyConnect.cardanoGetAddress({
    addressParameters: {
        addressType: 8,
        path: "m/44'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display base address of first cardano account:

```javascript
OneKeyConnect.cardanoGetAddress({
    addressParameters: {
        addressType: 0,
        path: "m/1852'/1815'/0'/0/0",
        stakingPath: "m/1852'/1815'/0'/2/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display pointer address of first cardano account:

```javascript
OneKeyConnect.cardanoGetAddress({
    addressParameters: {
        addressType: 4,
        path: "m/1852'/1815'/0'/0/0",
        certificatePointer: {
            blockIndex: 1,
            txIndex: 2,
            certificateIndex: 3,
        },
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display enterprise address of first cardano account:

```javascript
OneKeyConnect.cardanoGetAddress({
    addressParameters: {
        addressType: 6,
        path: "m/1852'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Display reward address of first cardano account:

```javascript
OneKeyConnect.cardanoGetAddress({
    addressParameters: {
        addressType: 14,
        path: "m/1852'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 1,
});
```

Return a bundle of cardano addresses without displaying them on device:

```javascript
OneKeyConnect.cardanoGetAddress({
    bundle: [
        // byron address, account 1, address 1
        {
            addressParameters: {
                addressType: 8,
                path: "m/44'/1815'/0'/0/0",
            },
            protocolMagic: 764824073,
            networkId: 1,
            showOnOneKey: false
        },
        // base address with staking key hash, account 1, address 1
        {
            addressParameters: {
                addressType: 0,
                path: "m/1852'/1815'/0'/0/0",
                stakingKeyHash: '1bc428e4720702ebd5dab4fb175324c192dc9bb76cc5da956e3c8dff',
            },
            protocolMagic: 764824073,
            networkId: 1,
            showOnOneKey: false
        },
        // byron address, account 2, address 3, testnet
        {
            addressParameters: {
                addressType: 8,
                path: "m/44'/1815'/1'/0/2",
            },
            protocolMagic: 42,
            networkId: 0,
            showOnOneKey: false
        },
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

const result = await OneKeyConnect.cardanoGetAddress({
    addressParameters: {
        addressType: 8,
        path: "m/44'/1815'/0'/0/0",
    },
    protocolMagic: 764824073,
    networkId: 0,
    address: "Ae2tdPwUPEZ5YUb8sM3eS8JqKgrRLzhiu71crfuH2MFtqaYr5ACNRdsswsZ",
});
// dont forget to hide your custom UI after you get the result!
```

#### Result

Result with only one address

```javascript
{
    success: true,
    payload: {
        addressParameters: {
            addressType: number,
            path: Array<number>, // hardend path
            stakingPath?: Array<number>, // hardend path
            stakingKeyHash?: string,
            certificatePointer?: {
                blockIndex: number,
                txIndex: number,
                certificatePointer: number,
            }
        }
        serializedPath: string,
        serializedStakingPath?: string,
        protocolMagic: number,
        networkId: number,
        address: string,
    }
}
```

Result with bundle of addresses

```javascript
{
    success: true,
    payload: [
        {
            addressParameters: {
                addressType: number,
                path: Array<number>, // hardend path
                stakingPath?: Array<number>, // hardend path
                stakingKeyHash?: string,
                certificatePointer?: {
                    blockIndex: number,
                    txIndex: number,
                    certificatePointer: number,
                }
            }
            serializedPath: string,
            serializedStakingPath?: string,
            protocolMagic: number,
            networkId: number,
            address: string,
        },
        {
            addressParameters: {
                addressType: number,
                path: Array<number>, // hardend path
                stakingPath?: Array<number>, // hardend path
                stakingKeyHash?: string,
                certificatePointer?: {
                    blockIndex: number,
                    txIndex: number,
                    certificatePointer: number,
                }
            }
            serializedPath: string,
            serializedStakingPath?: string,
            protocolMagic: number,
            networkId: number,
            address: string,
        },
        {
            addressParameters: {
                addressType: number,
                path: Array<number>, // hardend path
                stakingPath?: Array<number>, // hardend path
                stakingKeyHash?: string,
                certificatePointer?: {
                    blockIndex: number,
                    txIndex: number,
                    certificatePointer: number,
                }
            }
            serializedPath: string,
            serializedStakingPath?: string,
            protocolMagic: number,
            networkId: number,
            address: string,
        },
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

\<Playground initValue={ `OneKeyConnect.cardanoGetAddress({ addressParameters: { addressType: 8, path: "m/44'/1815'/0'/0/0", }, protocolMagic: 764824073, networkId: 1, });`} />
