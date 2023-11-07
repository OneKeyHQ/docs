# Search Devices

## Search device

Searches for connected devices and returns the search results to the developer as an array. \
\
In the case of USB connections, the returned data already contains device details. \
\
In the case of Bluetooth device search, the data returned contains only the device name and device connectId, and the developer selects the device that needs to be paired before getting the device information.

```typescript
const response = await HardwareSDK.searchDevices();
```

### Params

* empty

### Example

```typescript
HardwareSDK.searchDevices().then(result => {
    console.log(`device list: ${result}`)
});
```

Result

```typescript
{
    success: true,
    payload: [
        {
            "connectId": string, // device connection id
            "uuid": string, // device unique id 
            "deviceType": string, // device id, this id may change with device erasure, only returned when using the @onekeyfe/hd-web-sdk library.
            "deviceId": string, // device type, 'classic' | 'mini' | 'touch' | 'pro'
            "path": string, // path of bridge when usb is connected
            "name": string, // bluetooth name for the device
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
        code: number // error code
    }
}
```

