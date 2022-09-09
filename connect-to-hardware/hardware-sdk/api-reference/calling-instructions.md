# Calling Instructions

## Request

The generic entry for the method is referenced here: [Generic Parameters](general-parameters.md).

## Response

The return type of every method except the event listener is Promise, which means that when the state of the Promise object changes, the response to the method execution is retrieved. \
\
The response object looks like this 👇 The data structure of

```typescript
export interface Unsuccessful {
  success: false;
  payload: { error: string; code?: string | number };
}

export interface Success<T> {
  success: true;
  payload: T;
}

export type Response<T> = Promise<Success<T> | Unsuccessful>;
```

You can confirm whether the method was executed successfully by whether `response.success` is true or false.

## Error

If the method execution fails, you can check the `error` message in the `payload` to see the error. The error code is distinguished in detail in `response.payload.code`, and we also provide errorCode related constants in `@onekeyfe/hd-shared` library for you to determine and view the error type.

## Example

Get the address of BTC

```typescript
HardwareSDK.btcGetAddress('OneKey21042004483', '0B961C0007C7923D5B1D3341', {
  path: 'm/44'/0'/0'/0/0',
  coin: 'btc',
  showOnOneKey: false
});

```

### Results

```typescript
{
  event: "RESPONSE_EVENT",
  id: 9,
  payload: {
    address: "12rZ8ma1fUaXpDw7Nw5adHSvkfKtqKjJ16",
    path: "m/44'/0'/0'/0/0" 
  },
  success: true,
  type: "RESPONSE_EVEN
}
```

### Error

```javascript
{
  payload: {
    code: 0,
    error: "Not a valid path"
  },
  success: false
}
```
