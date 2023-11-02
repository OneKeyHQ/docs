# deviceWipe

## Wipe device

Reset device to factory defaults and remove all private data.

```typescript
const result = await HardwareSDK.wipeDevice(connectId);
```

## Params

\
empty

## Example

```typescript
HardwareSDK.wipeDevice(connectId);
```

## Result

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
