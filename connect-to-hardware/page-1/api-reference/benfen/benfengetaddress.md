# benfenGetAddress

### Benfen: get address

Display requested address derived by given BIP32 path on device and returns it to caller. User is asked to confirm the export on OneKey.

**Use requirement**

* Firmware version required
  * pro: 4.12.0

```typescript
const result = await HardwareSDK.benfenGetAddress(connectId, deviceId, params);
```

#### Params

**Optional common params**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `showOnOneKey` - _optional_ `boolean` whether to display address on device

#### Returns

* success: boolean - True if successful
* payload:
  * address: string - Derived BFC format address
  * path: string - BIP32 path used
  * pub?: string - Public key if available

Example:

```typescript
const params = {
  path: "m/44'/728'/0'/0'/0'",
  showOnOneKey: false
};

// Single address
const response = await HardwareSDK.benfenGetAddress(connectId, deviceId, params);

// Batch addresses
const batchParams = {
  bundle: [
    {
      path: "m/44'/728'/0'/0'/0'",
      showOnOneKey: false
    },
    {
      path: "m/44'/728'/1'/0'/0'",
      showOnOneKey: false
    }
  ]
};
const batchResponse = await HardwareSDK.benfenGetAddress(connectId, deviceId, batchParams);
```

**Result**

Result with only one address

```typescript
{
    success: true,
    payload: {
        address: string,     // displayed address
        pub: string,
        path: Array<number> // hardended path
    }
}
```

Result with bundle of addresses sorted by FIFO

```typescript
{
    success: true,
    payload: [
        { address: string, pub: string, path: Array<number> }, // account 1
        { address: string, pub: string, path: Array<number> }, // account 2
        { address: string, pub: string, path: Array<number> }  // account 3
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
