# firmwareUpdate

## Firmware update

Update stm32 firmware / Bluetooth firmware version.\
\
Firmware upgrades are supported in two ways:

* Use the firmware upgrade resource on the OneKey server and simply pass in the version number.&#x20;
* Use the user-specified binary data to upgrade the firmware. Use this method with caution; incorrect data can result in hardware data loss.

```typescript
const response = await HardwareSDK.firmwareUpdate(connectId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `updateType` — _required_ `'firmware' | 'ble'` firmware upgrade type
* `binary` — _optional_ `ArrayBuffer` firmware binary data when you upgrade with a custom file
* `version` — _optional_ `Array` specifying the firmware version number will match the version information in the resource list provided by OneKey

### Example

```typescript
const response = await HardwareSDK.firmwareUpdate(connectId, {
    updateType: 'firmware',
    version: [2, 10, 0]
});
```

Result

```typescript
{
  success: true,
  payload: {
      message: string
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
