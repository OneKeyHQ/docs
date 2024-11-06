# scdoGetAddress

#### Use requirement

* Firmware version required
  * Touch: 4.10.0

#### SCDO: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.scdoGetAddress(connectId, deviceId, {
    path: "m/44'/541'/0'/0/0",
    showOnOneKey: true
});
```

**Params**

Optional common params

**Exporting single address**

* `path` — _required_ `string | Array<number>` minimum length is 3. read more
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

**Example**

```typescript
// Get single address
const response = await HardwareSDK.scdoGetAddress(connectId, deviceId, {
    path: "m/44'/541'/0'/0/0",
    showOnOneKey: false
});

// Get multiple addresses
const bundleResponse = await HardwareSDK.scdoGetAddress(connectId, deviceId, {
    bundle: [
        {
            path: "m/44'/541'/0'/0/0",
            showOnOneKey: false
        },
        {
            path: "m/44'/541'/1'/0/0", 
            showOnOneKey: false
        }
    ]
});
```

Result with single address

```typescript
{
    success: true,
    payload: {
        address: string,
        path: string
    }
}
```

Result with bundle of addresses

```typescript
{
    success: true,
    payload: [
        { address: string, path: string }, // account 1
        { address: string, path: string }  // account 2
    ]
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code: number  // error code
    }
}
```
