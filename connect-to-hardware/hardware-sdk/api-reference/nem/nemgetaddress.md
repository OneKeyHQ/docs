# nemGetAddress

### Use requirement

* Firmware version required
  * Touch: 4.4.0
  * Classic/Mini: 3.2.0

## NEM: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

```typescript
const response = await HardwareSDK.nemGetAddress(connectId, deviceId, params)
```

### Params

[Optional common params](../common-params.md)

**Exporting single address**

* `path` — _required_ `string | Array<number>`  minimum length is 5. [more information](../path.md)
* `network`— _optional_ `number` Network id. The default value is `MAINNET(104)`
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Display address of first nem account:

```typescript
HardwareSDK.nemGetAddress(connectId, deviceId, {
    path: "m/44'/43'/0'/0/0"
});
```

Return a bundle of aptos addresses without displaying them on device:

```typescript
HardwareSDK.nemGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/44'/43'/0'/0/0", showOnOneKey: false }, // account 1
        { path: "m/44'/43'/1'/0/0", showOnOneKey: false }, // account 2
        { path: "m/44'/43'/2'/0/0", showOnOneKey: false }  // account 3
    ]
});
```

#### Result

Result with only one address

```typescript
{
    success: true,
    payload: {
        address: string,     // displayed address
        path: Array<number> // hardended path
    }
}
```

Result with bundle of addresses sorted by FIFO

```typescript
{
    success: true,
    payload: [
        { address: string, path: Array<number> }, // account 1
        { address: string, path: Array<number> }, // account 2
        { address: string, path: Array<number> }  // account 3
    ]
}
```

Error

```
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
