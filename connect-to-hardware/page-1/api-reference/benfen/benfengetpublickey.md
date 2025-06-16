# benfenGetPublicKey

Benfen: get public key

Get public key from device using BIP32 path.



**Use requirement**

* Firmware version required
  * pro: 4.12.0

```typescript
const result = await HardwareSDK.benfenGetPublicKey(connectId, deviceId, {
  path: "m/44'/728'/0'/0'/0'",
  showOnOneKey: false
});
```

#### Params

**Optional common params**

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `showOnOneKey` - _optional_ `boolean` whether to display public key on device

Returns:

* success: boolean - True if successful
* payload:
  * path: string - BIP32 path used
  * pub: string - Public key in hex format

Example:

```typescript
const params = {
  path: "m/44'/728'/0'/0'/0'",
  showOnOneKey: false
};

// Single public key
const response = await HardwareSDK.benfenGetPublicKey(connectId, deviceId, params);

// Batch public keys
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
const batchResponse = await HardwareSDK.benfenGetPublicKey(connectId, deviceId, batchParams);
```

