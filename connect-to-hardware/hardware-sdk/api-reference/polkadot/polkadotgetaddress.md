# polkadotGetAddress

## Requirements

* Minimum firmware versions
  * Touch: 4.3.0
  * Classic/Mini: 3.0.0
* Joystream Chain Minimum firmware versions
  * Touch: 4.7.0
  * Classic/Mini: 3.6.0
* Manta Chain Minimum firmware versions
  * Touch: 4.9.0
  * Classic/Mini: 3.7.0

## Polkadot: get address

Derives and returns the address at the specified path. Use `showOnOneKey` to request confirmation on the device.

```typescript
const result = await HardwareSDK.polkadotGetAddress(connectId, deviceId, params);
```

## Params

[**Optional common params**](../common-params.md)

### Single address

* `path` — _required_ `string | Array<number>` SDK minimum length is `3`. [read more](../path.md)
* `prefix` - _required_ `number`. SS58 address prefix; use `0` for Polkadot.
* `network` - _required_ `string` Network name, such as `polkadot`.
* `showOnOneKey` — _optional_ `boolean` Whether to display the address on the device. Defaults to `true`.

#### Multiple addresses

* `bundle` - array of objects with `path`, `showOnOneKey`, `prefix`, and `network` fields

### Example

Return address of first Polkadot account:

```typescript
HardwareSDK.polkadotGetAddress(connectId, deviceId, {
    path: "m/44'/354'/0'/0'/0'",
    showOnOneKey: true,
    prefix: 0,
    network: "polkadot"
});
```

Return a bundle of addresses for multiple Polkadot accounts:

```typescript
HardwareSDK.polkadotGetAddress(connectId, deviceId, {
    bundle: [
        { path: "m/44'/354'/0'/0'/1'", prefix: 0, network: "polkadot" }, // account 1
        { path: "m/44'/354'/0'/0'/2'", prefix: 0, network: "polkadot" }, // account 2
        { path: "m/44'/354'/0'/0'/3'", prefix: 0, network: "polkadot" }  // account 3
    ]
});
```

### Result

Single-address response

```typescript
{
    success: true,
    payload: {
        path: string, // serialized BIP32 path
        address: string,   // address
        publicKey: string, // publicKey
    }
}
```

Batch address response

```typescript
{
    success: true,
    payload: [
        { path, address, publicKey }, // account 1
        { path, address, publicKey }, // account 2
        { path, address, publicKey }, // account 3
    ]
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code?: string | number // error code
    }
}
```
