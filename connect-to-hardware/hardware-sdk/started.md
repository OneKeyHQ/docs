# Started

This guide provides clear and concise steps to seamlessly integrate and fully utilize our hardware SDK.

## **Step 0: Try** Online debugging tool

#### Step 1: Install bridge

The web terminal can communicate with hardware only after a hardware bridge is installed.

[Install Hardware Bridge](https://onekey.so/download?client=bridge)

#### Setp 2: Debug

You can use a USB connection to the device for debugging APIs.

[Online Debugging Tool](https://hardware-example.onekeytest.com/)



## **Step 1: Selector and Initialize the SDK**

1. Select the appropriate version of the SDK for your platform. [See SDK Platform Selection Guide](install-sdk.md).
2. Download and install the latest version of the SDK for timely technical support.

## **Step 2: Event Configuration**

After the hardware is successfully connected:

1. Configure the necessary global events (Events).
2. Handle events, for example:
   * `FIRMWARE_EVENT` is pushed when there is a firmware update.
   * Entering the hardware unlock PIN code in the software is implemented through the corresponding `EVENT`.
   * Requests requiring hardware confirmation will also inform the client through `EVENT`, like opening or closing confirmation windows.

By default, the device's PIN code input is handled by the software. if you need the hardware to handle related EVENTs, additional steps are required.

To ensure that you can fully understand and correctly handle these events, we recommend that you thoroughly refer to our [Event documentation](config-event.md).

## **Step 3: API Invocation**

Before using the API, ensure:

1. You understand the API call instructions and common parameters. See [API Call Instructions](install-sdk.md#initialization).
2. Invoke the API, including the common parameters ([Common Params](api-reference/common-params.md)).
3. Select the appropriate API based on the hardware firmware version. See [API Documentation](../air-gap-sdk/api-reference/).

#### **Response and Error Handling**

* The response type is `Promise`.
* A successful method returns the `Success` type; a failure returns the `Unsuccessful` type.
* Use `response.success` to determine if the method executed successfully.
* In case of failure, check the error information in `payload` and the error code in `response.payload.code`. For a list of error codes, see [Error Code List](api-reference/error-code.md).

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

## **Step 4:** How to Start the Business

First, you need to understand [Common Params](api-reference/common-params.md) and be clear about ConnectId and DeviceId, as almost every method in subsequent business will require them. Therefore, you need to call the [getFeatures API](api-reference/basic-api/get-features.md) method to save the relevant information.

Thus, the normal process for adding a new device is:

1. Use [searchDevice](api-reference/basic-api/search-devices.md) to find nearby devices.
   1. In the returned results, there will be information such as `connectId`, `deviceType`, and `name` that you need to save. For USB devices, there will also be a `DeviceId`.
   2. If it is a Bluetooth device, you will need to additionally use the [getFeatures](api-reference/basic-api/get-features.md) to obtain `DeviceId` relevant information and persistently save it.&#x20;
2. Later, for other business operations, you only need to call the relevant APIs and pass in the ConnectId and DeviceId.

## Development SDK

<pre class="language-bash"><code class="lang-bash"><strong>git clone https://github.com/OneKeyHQ/hardware-js-sdk.git
</strong><strong>cd hardware-js-sdk
</strong>yarn &#x26;&#x26; yarn bootstrap

# build
yarn build
<strong>
</strong># start hd-web-sdk
cd packages/hd-web-sdk &#x26;&#x26; yarn dev
</code></pre>
