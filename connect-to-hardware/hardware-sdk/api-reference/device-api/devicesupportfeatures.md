# deviceSupportFeatures

### Detect hardware supported features.

```typescript
const response = await HardwareSDK.deviceSupportFeatures(connectId);
```

### Params

[Optional common params](../../common-params.md)

### Example

```typescript
const response = await HardwareSDK.deviceSupportFeatures(connectId);
```

Result

```typescript
{
    success: true,
    payload: {
      inputPinOnSoftware: boolean, // You can unlock the device on the software side
      modifyHomescreen: boolean, // Modify wallpaper 
      device: Device // Device info
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
