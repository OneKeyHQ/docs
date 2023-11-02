# checkBridgeStatus

## Check OneKey bridge client status

Check the operational status of the hardware connection tool `Bridge`.

```typescript
const response = await HardwareSDK.checkBridgeStatus();
```

### Params

* empty

### Example

```typescript
const response = await HardwareSDK.checkBridgeStatus();
```

Result

```typescript
{
    success: true,
    payload: boolean
}
```

Error

```
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
