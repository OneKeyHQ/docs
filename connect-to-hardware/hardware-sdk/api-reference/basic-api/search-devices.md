# Search Devices

## Search device

Returns the devices discovered by the selected transport. Let the user choose which device to connect to. USB discovery may include initialized device details; Bluetooth discovery can return only routing and advertisement information until the device is connected.

```typescript
const response = await HardwareSDK.searchDevices();
```

### Params

No parameters.

### Example

```typescript
HardwareSDK.searchDevices().then(result => {
    if (result.success === true) {
        const devices = result.payload;
        // Display device names for selection. Avoid logging full device objects.
    } else {
        throw new Error(result.payload.error);
    }
});
```

Result

```typescript
{
    success: true,
    payload: [
        {
            "connectId": string | null, // transport routing identifier
            "uuid": string, // may be empty before device initialization
            "deviceType": string, // device family; may be unknown during discovery
            "deviceId": string | null, // initialized device identity, not the connection ID
            "name": string, // display name or Bluetooth advertisement name
        },
    ]
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
