# Pin

This document provides a comprehensive guide to the PIN feature for all users and developers, explaining its security model and technical integration.

### Part 1: Core Concepts (For Everyone) <a href="#part-1-core-concepts-for-everyone" id="part-1-core-concepts-for-everyone"></a>

#### 1. What is a PIN? A Lock for Your Device <a href="#id-1-what-is-a-pin-a-lock-for-your-device" id="id-1-what-is-a-pin-a-lock-for-your-device"></a>

A PIN (Personal Identification Number) is a numeric password that acts as a physical lock for your hardware wallet. It is the first line of defense against unauthorized access. Before any sensitive operation—such as signing a transaction, exporting a public key, or accessing settings—can be performed, the device must first be unlocked with the correct PIN.

A PIN is fundamentally different from a Passphrase:

* **PIN**: Secures the **device** itself from unauthorized use.
* **Passphrase**: Secures your **accounts** by creating hidden wallets.

#### 2. The "Blind PIN" Security Model <a href="#id-2-the-blind-pin-security-model" id="id-2-the-blind-pin-security-model"></a>

To protect against malware on a host computer (like keyloggers or screen recorders), the SDK uses a "Blind PIN" entry system for software-based input.

The core principle is: **The software application never knows the user's real PIN.**

Here's the workflow:

1. **Hardware Scrambles**: The hardware device's screen displays a numeric keypad where the numbers are deliberately scrambled. This layout is different each time.
2. **Software is "Blind"**: The software application displays a standard, fixed keypad (e.g., 1-9 in a grid). It has no knowledge of the scrambled layout on the hardware.
3. **User Enters by Position**: The user looks at their hardware screen to find their number and then clicks the corresponding _position_ on the software screen.
4. **Software Sends a "Transformed" PIN**: The software sends the PIN generated from its own fixed layout to the hardware. For example, if the user's PIN is `39`, the software might send `73` based on the positions clicked.
5. **Hardware Verifies**: The hardware receives the transformed PIN (`73`), internally translates it back to the real PIN (`39`) based on the layout it showed, and verifies it.

This ensures that even if an attacker compromises the host computer, they cannot steal the user's actual PIN.

***

### Part 2: Developer Integration Guide <a href="#part-2-developer-integration-guide" id="part-2-developer-integration-guide"></a>

This section provides a detailed walkthrough for developers integrating the OneKey PIN feature.

#### 1. The PIN Request Lifecycle (A Reactive Model) <a href="#id-1-the-pin-request-lifecycle-a-reactive-model" id="id-1-the-pin-request-lifecycle-a-reactive-model"></a>

Unlike Passphrase, which allows for proactively declaring an empty passphrase, PIN interaction is **always reactive**. This is because the PIN is a device-level lock. If the device is locked, no operation can proceed until it's unlocked. The lifecycle is straightforward:

1. **API Call**: Your application calls a protected SDK method (e.g., `HardwareSDK.btcGetAddress`).
2. **Hardware Check**: The SDK communicates with the hardware and finds it's in a locked state.
3. **Event Fired**: The SDK automatically pauses the execution of the original API call and emits a `UI_REQUEST.REQUEST_PIN` event.
4. **Application Responds**: Your application's global listener catches this event and displays a PIN entry UI. Based on the user's action, it sends a response back via `HardwareSDK.uiResponse`.
5. **Execution Resumes**: The SDK receives the response. If the PIN is correct (or on-device entry is chosen), the device unlocks, and the original, paused API call (`btcGetAddress`) automatically resumes and completes. If the response is `null`, the original call is aborted.

#### 2. Implementing the Event Listener <a href="#id-2-implementing-the-event-listener" id="id-2-implementing-the-event-listener"></a>

A robust integration requires a centralized event listener that can handle SDK events and route them to the appropriate UI components. This listener should be initialized when your application starts.

**Example: A complete event handler setup** This example shows how a listener can delegate the PIN request to a hypothetical UI manager, which would then be responsible for showing the dialog and wiring up the callbacks.

```javascript
// In your application's main setup or a dedicated event handler module
import { UI_EVENT, UI_REQUEST, UI_RESPONSE } from '@onekeyfe/hd-core';
// uiManager is a hypothetical module you would create to manage showing/hiding dialogs
import { uiManager } from './UIManager'; 

HardwareSDK.on(UI_EVENT, (message) => {
  if (message.type === UI_REQUEST.REQUEST_PIN) {
    // The hardware is locked and needs a PIN.
    // Trigger your application's UI to open a PIN dialog.
    // This function would be responsible for showing a component like <PinDialog />
    // and passing it the necessary callbacks.
    uiManager.showPinDialog({
      // Callback for when the user enters a PIN on the software UI
      onPinSubmit: (transformedPin) => {
        HardwareSDK.uiResponse({
          type: UI_RESPONSE.RECEIVE_PIN,
          payload: transformedPin,
        });
      },
      // Callback for when the user clicks the "Enter on Device" button
      onDeviceInput: () => {
        HardwareSDK.uiResponse({
          type: UI_RESPONSE.RECEIVE_PIN,
          payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
        });
      },
      // Callback for when the user cancels the dialog
      onCancel: () => {
         HardwareSDK.uiResponse({
          type: UI_RESPONSE.RECEIVE_PIN,
          payload: null,
        });
      }
    });
  }
});
```

#### 3. Responding to PIN Events: The Scenarios <a href="#id-3-responding-to-pin-events-the-scenarios" id="id-3-responding-to-pin-events-the-scenarios"></a>

When you receive a `REQUEST_PIN` event, your application must provide a response using `HardwareSDK.uiResponse`. Here are the possible scenarios.

**Scenario 1: Entering the PIN on the Hardware Device (Most Secure)** This method delegates the entire input process to the hardware device's trusted screen and buttons.

**Implementation:**

```javascript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PIN,
  // This special string instructs the SDK to activate on-device input.
  payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE',
});
```

**Scenario 2: Entering the PIN via Software Input (Blind PIN)** This method uses your application's UI for input, following the "Blind PIN" security model. Your application provides the UI, and the user enters the PIN by matching positions between the hardware screen and your UI.

**Implementation:**

```javascript
// This value is the "transformed" PIN that your application
// derived from the user's positional clicks on your UI's fixed keypad.
const usersTransformedPin = '7319'; // Example value

HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PIN,
  payload: usersTransformedPin,
});
```

**Scenario 3: Canceling a PIN Request** If the user decides to cancel the operation (e.g., by closing the PIN dialog), you must notify the SDK by sending `null` as the payload. This will abort the pending hardware operation.

**Implementation:**

```javascript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PIN,
  payload: null,
});
```

#### 4. Understanding Device State: Unlocked vs. Locked <a href="#id-4-understanding-device-state-unlocked-vs-locked" id="id-4-understanding-device-state-unlocked-vs-locked"></a>

It's crucial to understand that PIN management is about device state, not session state like the `passphraseState`.

* **No `pinState`**: There is no concept of a `pinState` to be passed in subsequent API calls.
* **Unlocked State**: Once a device is successfully unlocked with a PIN, it remains in an "unlocked" state.
* **Automatic Lock**: The device will automatically re-lock after a pre-configured period of inactivity or when it is disconnected/powered off.
* **Seamless Operations**: While the device is in the unlocked state, all subsequent protected API calls will execute immediately without firing a `REQUEST_PIN` event. Your application does not need to do anything extra. The event listener will simply not be triggered again until the device re-locks.
