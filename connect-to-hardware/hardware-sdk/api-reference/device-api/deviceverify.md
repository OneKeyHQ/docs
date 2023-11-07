# deviceVerify

## Verify device

Get the hardware signature information cert and signature fields from the device and submit them to the OneKey server to verify the reliability of the device. Reading signature data requires confirmation at the device.

```typescript
const result = await HardwareSDK.deviceVerify(connectId, params);
```

## Params

[**Optional common params**](../common-params.md)\


* `dataHex` — _optional_ `string` A random string of hex string, such as a timestamp passed in hex string.

## Example

```typescript
HardwareSDK.deviceVerify(connectId, {
    dataHex: '183533a058f'
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
        code: number // error code
    }
}
```
