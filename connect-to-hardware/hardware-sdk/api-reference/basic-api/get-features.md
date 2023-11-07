# Get Features

## Get device features

Get all attributes of the device.

```typescript
const response = await HardwareSDK.getFeatures(connectId);
```

> If the screen is displaying other content, you can also call this method to return to the home page.

### Params

[**Optional common params**](../common-params.md)

### Example

```typescript
const response = await HardwareSDK.getFeatures(connectId);
```

Result

```typescript
{
    success: true,
    payload: {
        major_version: number, // major version number
        minor_version: number, // middle version number
        patch_version: number, // patch version number
        bootloader_mode: null,
        device_id: string,
        pin_protection: boolean,
        passphrase_protection: boolean, // enable passphrase
        language: string,
        label: string, // device name
        initialized: boolean, // device initialization status
        model: string,
        safety_checks: string,
        ble_name: string, // bluetooth name
        ble_ver: string, // bluetooth firmware version
        ble_enable: string,
        se_enable: boolean,
        se_ver: string,
        backup_only: boolean,
        onekey_version: string, // firmware Version
        onekey_serial: string, // device Serial Number
        bootloader_version: string, // bootloader version
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

