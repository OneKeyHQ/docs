# EvmGetAddress

## Ethereum: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is presented with a description of the requested key and asked to confirm the export on OneKey.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const response = await HardwareSDK.evmGetAddress(connectId, deviceId, params)
```
{% endtab %}
{% endtabs %}

### Params

****[**Optional common params**](../../hardware-api/method-list/commonparams.md)****

**Exporting single address**

* `path` — _required_ `string | Array<number>`  minimum length is 5. [more information](path.md)
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`

**Exporting bundle of addresses**

* `bundle` - `Array` of Objects with `path` and `showOnOneKey` fields

#### Example

Display address of first ethereum account:

```typescript
HardwareSDK.evmGetAddress({
    path: "m/44'/60'/0'/0/0"
});
```

Return a bundle of ethereum addresses without displaying them on device:

```typescript
HardwareSDK.evmGetAddress({
    bundle: [
        { path: "m/44'/60'/0'/0/0", showOnOneKey: false }, // account 1
        { path: "m/44'/60'/1'/0/0", showOnOneKey: false }, // account 2
        { path: "m/44'/60'/2'/0/0", showOnOneKey: false }  // account 3
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
