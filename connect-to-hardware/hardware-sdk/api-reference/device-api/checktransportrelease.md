# checkTransportRelease

## Check OneKey bridge release

Check the version update status of the hardware connection tool `Bridge`.

```typescript
const response = await HardwareSDK.checkTransportRelease();
```

### Params

* empty

### Example

```typescript
const response = await HardwareSDK.checkTransportRelease();
```

Result

```typescript
{
  success: true,
  payload: string, // local bridge version status, 'valid' or 'outdated'
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
