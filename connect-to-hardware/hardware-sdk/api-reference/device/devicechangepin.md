# deviceChangePin

## Change device pin

Change the device pin code.

```typescript
const result = await HardwareSDK.deviceChangePin(connectId, params);
```

## Params

****[**Optional common params**](../common-params.md)

* `remove` — _optional_ `boolean` request remove pin.

## Example

```typescript
HardwareSDK.deviceChangePin(connectId, {
    remove: false
});
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
