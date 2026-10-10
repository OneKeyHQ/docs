# deviceWipe

## Wipe device

Reset device to factory defaults and remove all private data.

```typescript
const result = await HardwareSDK.deviceWipe(connectId);
```

## Params

\
empty

## Example

```typescript
HardwareSDK.deviceWipe(connectId);
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
        code?: string | number // error code
    }
}
```
