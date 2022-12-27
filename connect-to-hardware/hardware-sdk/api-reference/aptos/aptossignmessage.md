# aptosSignMessage

## Aptos: sign message <a href="#ethereum-sign-message" id="ethereum-sign-message"></a>

Asks device to sign a message using the private key derived by given BIP32 path.

```typescript
const result = await HardwareSDK.aptosSignMessage(connectId, deviceId, params);
```

### Params

****[**Optional common params**](../common-params.md)****

* `path` — _required_ `string | Array<number>` minimum length is `3`. read more
* `payload` - `AptosMessagePayload` type
  * `address` - _optional  `string` _ of __ path Derived address
  * `chainId` - _optional  `string` _ of the id of the chain
  * `application` - _optional  `string` _ of DApp web site url
  * `nonce` - _required  `string` _ of random number
  * `message` - _required_ `string` message to sign text

### Example

```typescript
HardwareSDK.aptosSignMessage(connectId, deviceId, {
    path: "m/44'/637'/0'/0'/0",
    payload: {
        nonce: "1",
        message: "hello world"
    }
});
```

Result

```typescript
{
    success: true,
    payload: {
        path: string;
        fullMessage: string;
        signature: string;
        address: string;
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
