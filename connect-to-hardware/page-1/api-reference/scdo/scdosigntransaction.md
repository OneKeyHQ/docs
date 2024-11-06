# scdoSignTransaction

#### Use requirement

* Firmware version required
  * Touch: 4.10.0

#### SCDO: sign transaction

Signs transaction using the private key derived by given BIP32 path.

```typescript
const response = await HardwareSDK.scdoSignTransaction(connectId, deviceId, {
    path: "m/44'/541'/0'/0/0",
    nonce: "0x0",
    gasPrice: "0xbebc200",
    gasLimit: "0x5208", 
    to: "1S0118a02f993fc7a4348fd36b7f7a596948f02b31",
    value: "0xf4240",
    timestamp: "0",
    data: "0x", // optional
    txType: 0   // optional
});
```

**Params**

Optional common params

* `path` — _required_ `string | Array<number>` minimum length is 3. read more
* `nonce` — _required_ `string` transaction nonce in hex
* `gasPrice` — _required_ `string` transaction gas price in hex
* `gasLimit` — _required_ `string` transaction gas limit in hex
* `to` — _required_ `string` destination address
* `value` — _required_ `string` transaction value in hex
* `timestamp` — _optional_ `string` transaction timestamp
* `data` — _optional_ `string` transaction data in hex
* `txType` — _optional_ `number` transaction type

**Example**

```typescript
const response = await HardwareSDK.scdoSignTransaction(connectId, deviceId, {
    path: "m/44'/541'/0'/0/0",
    nonce: "0x0",
    gasPrice: "0xbebc200",
    gasLimit: "0x5208",
    to: "1S0118a02f993fc7a4348fd36b7f7a596948f02b31",
    value: "0xf4240",
    timestamp: "0"
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string,
        data_length: number,
        path: string
    }
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
