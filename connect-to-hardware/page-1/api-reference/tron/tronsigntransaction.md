# tonSignMessage

### TON: Sign transaction <a href="#cardano-sign-transaction" id="cardano-sign-transaction"></a>

Asks device to sign given transaction. User is asked to confirm all transaction details on OneKey.

```typescript
const response = await HardwareSDK.tonSignMessage(connectId, deviceId, params)
```

### Params

[**Optional common params**](../../../hardware-sdk/api-reference/common-params.md)

* `path` - _required_ `string | Array<number>` minimum length is `3`. read more
* `destination` - _required_ `TronTransaction` type
* `tonAmount` - _required_ `number`&#x20;
* `seqno` -  _required_ `number`&#x20;
* `expireAt` -  _required_ `number`&#x20;



### Example

```typescript
HardwareSDK.tonSignMessage(connectId, deviceId, {
    path: "m/44'/607'/0'",
    destination: "UQBYkuShkZzRYAWX_HrK3kFpeAixiRKd-K7QBXYxl9OBXM0_",
    tonAmount: 100,
    seqno: 0,
    expireAt: 1728713831896,
});
```

Result

```typescript
{
    success: true,
    payload: {
        signature: string;
        signning_message: string
        skip_validate: boolean; // Used to skip validation in [classic, classic 1s, mini, touch]
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
