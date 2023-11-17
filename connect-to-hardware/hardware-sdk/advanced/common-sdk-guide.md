# Common SDK Guide

## Started

This is a guide on how to integrate the OneKey SDK for iOS, Android, and Flutter.&#x20;

The following content requires you to have a basic understanding of NodeJS, Android, iOS, and Flutter.&#x20;

Below are the links to the relevant demos.

[iOS Demo Link](https://github.com/originalix/Hardware-Lowlevel-Communicate)

[Android Demo Link](low-level-transport-plugin.md)

[Flutter Demo Link](https://github.com/ByteZhang1024/OneKeyFlutterHardwareExample)



## JavaScript Runtime Environment

{% tabs %}
{% tab title="iOS" %}
For iOS, there are two options for the JavaScript runtime environment:

1. JavaScriptCore, recommended by iOS, with relevant documentation available at [Apple Developer Website](https://developer.apple.com/documentation/javascriptcore).
2. The WebView approach.

In the demo, the WebView solution is used.

After determining the runtime environment, a communication solution with WebView is also needed. In the demo, [WKWebViewJavascriptBridge](https://github.com/Lision/WKWebViewJavascriptBridge) is used for communication with the iOS native side.
{% endtab %}

{% tab title="Android" %}
For Android, there are two options for the JavaScript runtime environment:

1. The JavaScriptEngine recommended by Android Official, with relevant documentation available at [Android Developer Website](https://developer.android.com/develop/ui/views/layout/webapps/jsengine).
2. The WebView approach.

The JavaScriptEngine method is relatively new and requires a higher version of Android. Therefore, the demo still uses WebView for demonstration purposes.

After determining the runtime environment, we also need a communication solution with WebView. In the demo, [JSBridge-Android](https://github.com/smallbuer/JSBridge-Android) is used for communication with the Android native side.
{% endtab %}

{% tab title="Flutter" %}
For Flutter, there are two options for the JavaScript runtime environment:

* [Javascript engine](https://pub.dev/packages/flutter\_js) to use with flutter.
* The WebView approach.

Here, for consistency with other demos, the method of loading JavaScript using WebView was still chosen. You can try selecting others.
{% endtab %}
{% endtabs %}

## Create JavaScript Code

Create a folder and use `yarn init` to initialize a NodeJS project.

```bash
# Install via YARN
yarn add @onekeyfe/hd-common-connect-sdk @noble/hashes ripple-keypairs
```

It's necessary to install both `@noble/hashes` and `ripple-keypairs` as these libraries are dependencies required by the OneKey SDK.

Create `index.html` and `index.js` and package the JavaScript code. Since only simple JavaScript packaging is needed, [Parcel](https://parceljs.org/) is chosen over WebPack due to its simplicity and lack of complex configuration.

```
# Install via YARN
yarn add --dev parcel
```

For detailed configuration, you can refer to the web settings in the demo.

* [iOS web Demo Link](https://github.com/originalix/Hardware-Lowlevel-Communicate/tree/main/web)
* [Android web Demo Link](https://github.com/ByteZhang1024/OneKeyHardwareExample/tree/main/web)
* [Flutter Demo Link](https://github.com/ByteZhang1024/OneKeyFlutterHardwareExample)

These steps will help set up the JavaScript environment and integrate the necessary SDK and dependencies for the OneKey integration on iOS and Android platforms.

### Cnfigure the LowlevelPlugin

Next, configure the LowlevelPlugin. There is a document you can refer to for more information [Read more>>>](low-level-transport-plugin.md)



To forward `LowlevelPlugin` functionality to the Native side and return related results, follow these steps:

1. **Define Interface**: Define an interface in `LowlevelPlugin` for calling Native functionality.
2. **Implement Forwarding Mechanism**:
   * Capture the calls in `LowlevelPlugin` that need to be forwarded.
   * Forward the details of the call (such as method name, parameters, etc.) to the Native side.
3. **Native Side Processing**:
   * On the Native side, receive the request, parse the method name and parameters.
   * Execute the corresponding Native functionality.
4. **Return Results**:
   * Once the Native side has completed execution, return the results or error information back to `LowlevelPlugin`.
   * `LowlevelPlugin` then returns this information to the caller.

If unclear, you can refer to the related demos for guidance.

## Connection Device

{% tabs %}
{% tab title="Bluetooth" %}
The provided pseudocode outlines the steps for handling Bluetooth communication within a WebView environment in an Android or iOS application. Here's a breakdown of the key components:

**Bluetooth Parameters**

* `serviceUuid`: 00000001-0000-1000-8000-00805f9b34fb
* `writeCharacteristic`: 00000002-0000-1000-8000-00805f9b34fb
* `notifyCharacteristic`: 00000003-0000-1000-8000-00805f9b34fb

**Handling Bluetooth Scanning**

```
webview.addHandler("enumerate", (data: String?, function: CallBackFunction?) {
    // search Device
    // delay 3-5 seconds or wait until a device is scanned
    // use serviceUuid to filter for relevant devices
    val deviceList = searchResults.map {
        // Return the device's MacAddress and name to WebView using the following data structure
        LowLevelDevice(
            id = it.device.address,
            name = it.device.name ?: "",
        )
    }
    function?.onCallBack(deviceList)
})
```

**Handling Bluetooth Connection**

This method will be called each time an SDK method is invoked.

```
webview.addHandler("connect", (data: String?, function: CallBackFunction?) {
    // First, check and obtain Bluetooth usage permission, refer to relevant development documentation.
    
    // data will pass the uuid, which is the MacAddress of the Bluetooth device.
    // Connect to the device using the MacAddress
    
    // After a successful connection, obtain the Bluetooth device's Characteristics
    // The uuid of writeCharacteristic is 00000002-0000-1000-8000-00805f9b34fb
    // Used for sending data to the hardware
    
    // The uuid of notifyCharacteristic is 00000003-0000-1000-8000-00805f9b34fb
    // Used to receive data sent from the hardware
    
    // Store writeCharacteristic and notifyCharacteristic, and start listening to data from notifyCharacteristic
    
    // Inform WebView that the connection has been successful
    function?.onCallBack("success")
}
```

**Listening to `notifyCharacteristic` Data**:

```
notifyCharacteristic?.getNotifications()?.onEach {
    // Tell WebView all the data received from the device
    // Here, the assembly of multi-packet Bluetooth data is handled in JS, specific details can be found in the demo
    webview.callHandler(
        "monitorCharacteristic",
        HexUtil.toHex(it.value)
    )
}
```

**Handling Bluetooth Data Sending**:

```
webview.addHandler("send", (data: String?, function: CallBackFunction?) {   
    // data will pass the uuid, which is the MacAddress of the Bluetooth device and the specific data to be transmitted
    val uuid = 
    val data = 
    
    // Send data to the Bluetooth device
    writeCharacteristic?.write(data)
    
    // Inform WebView that the transmission is complete
    function?.onCallBack("success")
})
```

**Handling Bluetooth Disconnection**:

```
webview.addHandler("disconnect", (data: String?, function: CallBackFunction?) {
    // Simply disconnect
    // connection?.disconnect()
})
```

This pseudocode provides a framework for integrating Bluetooth functionalities within a WebView environment, allowing for communication between the web content and the native platform's Bluetooth capabilities. The actual implementation will depend on the specific requirements of the application and the characteristics of the connected Bluetooth device.
{% endtab %}

{% tab title="USB" %}
Coming soon
{% endtab %}
{% endtabs %}

Next, you can return to [Quickstart](../started.md) to view the relevant documentation for [Config Event](../config-event.md). Then, configure the Event in your JavaScript code.
