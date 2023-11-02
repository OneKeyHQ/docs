# deviceUpdateReboot

## Reboot device

Boot the device into bootloader mode

```typescript
const result = await HardwareSDK.deviceUpdateReboot(connectId);
```

## Params

* empty

## Example

```typescript
HardwareSDK.deviceUpdateReboot(connectId);
```

## Result

```typescript
{
    success: true,
    payload: {}
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
