# Get Passphrase State

## Get the passphraseState of the device

Requesting the API will prompt the user to enter a passphrase. This method will return a string result, which can be used as a `passphraseState` request parameter for subsequent APIs. The SDK will compare this parameter with the user's input. If the results match, the parameter will be cached to reduce user input and improve the user experience.

```typescript
const response = await HardwareSDK.getPassphraseState(connectId);
```

### Params

[**Optional common params**](../common-params.md)

### Example

```typescript
const response = await HardwareSDK.getPassphraseState(connectId);
```

Result

```
{
    success: true,
    payload: string
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
