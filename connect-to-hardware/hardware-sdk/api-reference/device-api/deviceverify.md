# deviceVerify

## Verify device

Retrieves the device certificate and a signature over a challenge. This method returns `cert` and `signature`; it does not itself submit them to a verification server. Reading this data requires device confirmation.

```typescript
const result = await HardwareSDK.deviceVerify(connectId, params);
```

## Params

[**Optional common params**](../common-params.md)\\

* `dataHex` — _optional_ `string` A random string of hex string, such as a timestamp passed in hex string,

## Example

```typescript
HardwareSDK.deviceVerify(connectId, {
    dataHex: challengeHex
});
```

## Result

```typescript
{
    success: true,
    payload: {
        cert: string
        signature: string
    }
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
