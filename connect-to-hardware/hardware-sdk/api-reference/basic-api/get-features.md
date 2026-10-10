# Get Features

## Get device features

Returns device firmware, identity, and configuration information. Fields vary by model, firmware, and protocol.

```typescript
const response = await HardwareSDK.getFeatures(connectId);
```

> This is a legacy API. Current SDK versions recommend [getDeviceState](https://developer.onekey.so/en/hardware-sdk/basic-api/get-device-state), whose device identifier is `payload.identity.deviceId`. Do not use an information request as a general-purpose command to navigate the device screen.

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
        minor_version: number, // minor version number
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
        ble_enable: boolean,
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
        code?: string | number // error code
    }
}
```
