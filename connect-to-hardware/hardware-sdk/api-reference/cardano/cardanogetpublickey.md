# cardanoGetPublicKey

## Use requirement

* Firmware version required
  * Touch: 4.1.0
  * Classic/Mini: 3.0.0

## Cardano: get public key

Retrieves BIP32-Ed25529 extended public derived by given BIP32-Ed25519 path. User is presented with a description of the requested key and asked to confirm the export.

```typescript
const result = await HardwareSDK.cardanoGetPublicKey(connectId, deviceId, params);
```

## Params

[**Optional common params**](../../common-params.md)

### Exporting single public key

* `path` — _required_ `string | Array<number>` minimum length is `1`. read more
* `showOnOneKey` — _optional_ `boolean` determines if address will be displayed on device. Default is set to `true`
* `derivationType` — _optional_ `CardanoDerivationType` enum. determines used derivation type. Default is set to ICARUS=1

#### Exporting bundle of public keys

* `bundle` - `Array` of Objects with `path`, `showOnOneKey` fields

### Example

Return public key of first cardano account:

```typescript
HardwareSDK.aptosGetPublicKey(connectId, deviceId, {
    path: "m/1852'/1815'/0'/0/0",
    showOnOneKey: true
});
```

Return a bundle of public keys for multiple cardano accounts:

```typescript
HardwareSDK.cardanoGetPublicKey(connectId, deviceId, {
    bundle: [
        { path: "m/1852'/1815'/0'/0/0", }, // account 1
        { path: "m/1852'/1815'/1'/0/0", }, // account 2
        { path: "m/1852'/1815'/2'/0/0", }  // account 3
    ]
});
```

### Result

Result with only one public key

```typescript
{
    success: true,
    payload: {
        path: Array<number>, // hardended path
        serializedPath: string,
        publicKey: string,
        node: HDPubNode,
    }
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
