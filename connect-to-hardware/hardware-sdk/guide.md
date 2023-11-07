# Guide

This guide provides clear and concise steps to seamlessly integrate and fully utilize our hardware SDK.

## **Step 1: Access and Initialize the SDK**

1. Select the appropriate version of the SDK for your platform. [See SDK Platform Selection Guide](quickstart.md).
2. Download and install the latest version of the SDK for timely technical support.

## **Step 2: Event Configuration**

After the hardware is successfully connected:

1. Configure the necessary global events (Events).
2. Handle events, for example:
   * `FIRMWARE_EVENT` is pushed when there is a firmware update.
   * Entering the hardware unlock PIN code in the software is implemented through the corresponding `EVENT`.
   * Requests requiring hardware confirmation will also inform the client through `EVENT`, like opening or closing confirmation windows.

For more information, please refer to the [Event Handling Documentation.](event.md)

## **Step 3: API Invocation**

Before using the API, ensure:

1. You understand the API call instructions and common parameters. See [API Call Instructions](https://app.gitbook.com/o/xrwuLsQQ99ktq7CL9Sjj/s/iaFYYqdr7FTQYgxCmg5J/\~/changes/127/connect-to-hardware/hardware-sdk/common-params).
2. Invoke the API, including the common parameters (Common Params).
3. Select the appropriate API based on the hardware firmware version. See [API Documentation](api-reference/).

#### **Response and Error Handling**

* The response type is `Promise`.
* A successful method returns the `Success` type; a failure returns the `Unsuccessful` type.
* Use `response.success` to determine if the method executed successfully.
* In case of failure, check the error information in `payload` and the error code in `response.payload.code`. For a list of error codes, see [Error Code List](error-code.md).

#### **Response Data Structure**

```typescript
export interface Unsuccessful {
  success: false;
  payload: { 
    error: string; // Specific error message
    code?: string | number; // Error code
  };
}

export interface Success<T> {
  success: true;
  payload: T;
}

export type Response<T> = Promise<Success<T> | Unsuccessful>;
```

### **Example: Getting a BTC Address**

```typescript
HardwareSDK.btcGetAddress('OneKey21042004483', '0B961C0007C7923D5B1D3341', {
  path: 'm/44\'/0\'/0\'/0/0',
  coin: 'btc',
  showOnOneKey: false
}).then(response => {
  if (response.success) {
    console.log(response.payload.address); // Handling success
  } else {
    console.error(response.payload.error); // Handling failure
  }
});
```

#### Successful Result

```typescript
{
  payload: {
    address: "12rZ8ma1fUaXpDw7Nw5adHSvkfKtqKjJ16",
    path: "m/44'/0'/0'/0/0" 
  },
  success: true
}
```

#### Error Information

```javascript
{
  payload: {
    code: -1,
    error: "Not a valid path"
  },
  success: false
}
```

