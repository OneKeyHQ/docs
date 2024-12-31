# checkFirmwareRelease

## Check firmware release

Check the current Bluetooth firmware version status of the device.

```typescript
const response = await HardwareSDK.checkBLEFirmwareRelease(connectId);
```

### Params

[Optional common params](../common-params.md)

### Example

```typescript
const response = await HardwareSDK.checkFirmwareRelease(connectId);
```

Result

```typescript
{
    success: true,
    payload: {
        status: string, // current firmware status, 'valid' | 'outdated' | 'required' | 'unknown' | 'none'
        // 'valid' means update is not required
        // 'outdated' means updates are available
        // 'required' means updates are required. 
        // 'unknown' and 'none' means there's no way to get the version number
        changelog: [], // update logs
        release: {     // latest version information
          required: boolean,
          version: Array<number>,
          url: string,
          webUpdate: string,
          changelog: Record<string, string>
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
