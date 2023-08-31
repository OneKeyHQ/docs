# btcGetAddress

## Bitcoin: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is asked to confirm the export on OneKey.

```typescript
const result = await HardwareSDK.btcGetAddress(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

#### Exporting single address

* `path` — _required_ `string | Array<number>` minimum length is `5`. [read more](../path.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `coin` - _optional_ `string` determines network definition specified in coins.json file. Coin `shortcut`, `name` or `label` can be used. If `coin` is not set API will try to get network definition from `path`.
* `multisig` - _optional_ MultisigRedeemScriptType, redeem script information (multisig addresses only)
* `scriptType` - _optional_ InputScriptType, address script type

#### Exporting bundle of addresses

* `bundle` - `Array` of Objects with `path`, `showOnOneKey`, `coin` fields

#### Example

Display third address of first bitcoin account:

```typescript
HardwareSDK.btcGetAddress(connectId, deviceId, {
    path: "m/49'/0'/0'/0/2",
    coin: "btc"
});
```

Return a bundle of addresses from first bitcoin account without displaying them on device:

```typescript
HardwareSDK.btcGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/49'/0'/0'/0/0", showOnOneKey: false }, // address 1
        { path: "m/49'/0'/0'/0/1", showOnOneKey: false }, // address 2
        { path: "m/49'/0'/0'/0/2", showOnOneKey: false }  // address 3
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

Result with bundle of addresses

```typescript
{
    success: true,
    payload: [
        { address: string, path: Array<number> }, // address 1
        { address: string, path: Array<number> }, // address 2
        { address: string, path: Array<number> }, // address 3
    ]
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
