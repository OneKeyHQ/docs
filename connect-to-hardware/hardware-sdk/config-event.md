# Event Configuration

The SDK uses an event-driven architecture to handle device interactions, UI requests, and state changes. This guide explains how to properly configure and handle these events.

## Event Subscription

Subscribe to SDK events using the `on` method:

```typescript
HardwareSDK.on(eventType: string, callback: (message: CoreMessage) => void);
```

### Event Types

```typescript
import {
  UI_EVENT,
  UI_REQUEST,
  UI_RESPONSE,
  DEVICE_EVENT,
  FIRMWARE_EVENT,
  CoreMessage
} from '@onekeyfe/hd-core';
```

#### Core Event Categories
- `UI_EVENT`: User interface interactions
- `DEVICE_EVENT`: Device state changes
- `FIRMWARE_EVENT`: Firmware updates and status

### Event Message Structure

```typescript
interface CoreMessage {
  event: string;    // Event category
  type: string;     // Specific event type
  payload: any;     // Event data
}
```

## UI Event Handling

UI events require interaction with the user or device. Handle these properly to ensure good UX.

```typescript
// Device state tracking
let isDeviceProcessing = false;
let currentDeviceId: string | null = null;

// UI event handler
HardwareSDK.on(UI_EVENT, async (message: CoreMessage) => {
  try {
    switch (message.type) {
      case UI_REQUEST.REQUEST_PIN:
        await handlePinRequest();
        break;
      case UI_REQUEST.REQUEST_PASSPHRASE:
        await handlePassphraseRequest();
        break;
      case UI_REQUEST.REQUEST_BUTTON:
        handleButtonRequest();
        break;
      case UI_REQUEST.CLOSE_UI_WINDOW:
        handleCloseUI();
        break;
      // Handle other UI events...
    }
  } catch (error) {
    console.error('UI event handling error:', error);
    handleError(error);
  }
});

// Implementation examples
async function handlePinRequest() {
  isDeviceProcessing = true;
  try {
    // Show PIN entry UI or device prompt
    await HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PIN,
      payload: '@@ONEKEY_INPUT_PIN_IN_DEVICE'
    });
  } finally {
    isDeviceProcessing = false;
  }
}

async function handlePassphraseRequest() {
  isDeviceProcessing = true;
  try {
    // Show passphrase entry UI or device prompt
    await HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PASSPHRASE,
      payload: {
        value: '',
        passphraseOnDevice: true,
        save: false
      }
    });
  } finally {
    isDeviceProcessing = false;
  }
}

function handleButtonRequest() {
  isDeviceProcessing = true;
  // Show "Confirm on device" UI
  showConfirmationUI();
}

function handleCloseUI() {
  isDeviceProcessing = false;
  // Clean up UI state
  hideAllPrompts();
}
```

## Device Event Handling

Monitor device connection state and features:

```typescript
HardwareSDK.on(DEVICE_EVENT, (message: CoreMessage) => {
  switch (message.type) {
    case 'DEVICE_CONNECT':
      handleDeviceConnect(message.payload);
      break;
    case 'DEVICE_DISCONNECT':
      handleDeviceDisconnect();
      break;
    case 'FEATURES':
      handleDeviceFeatures(message.payload);
      break;
    // Handle other device events...
  }
});

function handleDeviceConnect(device: any) {
  currentDeviceId = device.deviceId;
  // Update UI for connected state
}

function handleDeviceDisconnect() {
  isDeviceProcessing = false;
  currentDeviceId = null;
  // Show reconnection UI if needed
}

function handleDeviceFeatures(features: any) {
  // Handle device capabilities
  // Update UI based on supported features
}
```

## Firmware Event Handling

Monitor firmware updates and status:

```typescript
HardwareSDK.on(FIRMWARE_EVENT, (message: CoreMessage) => {
  switch (message.type) {
    case 'firmware-release-info':
      handleFirmwareInfo(message.payload);
      break;
    case 'ble-firmware-release-info':
      handleBLEFirmwareInfo(message.payload);
      break;
    // Handle other firmware events...
  }
});
```

## Best Practices

1. Error Handling
   - Always wrap event handlers in try/catch
   - Clean up state in finally blocks
   - Handle timeout scenarios

2. State Management
   - Track device processing state
   - Handle concurrent operations
   - Clean up on disconnect

3. UI Feedback
   - Show clear progress indicators
   - Handle timeouts gracefully
   - Provide clear error messages

4. Event Cleanup
   ```typescript
   // Remove event listeners when done
   const handler = (message) => { /* ... */ };
   HardwareSDK.on(UI_EVENT, handler);

   // Cleanup
   HardwareSDK.off(UI_EVENT, handler);
   ```

## Event Reference

### UI Events
- `REQUEST_PIN`: PIN code entry required
- `REQUEST_PASSPHRASE`: Passphrase entry required
- `REQUEST_BUTTON`: Device button confirmation needed
- `CLOSE_UI_WINDOW`: Close all UI prompts
- `BLUETOOTH_PERMISSION`: BLE permission required
- `LOCATION_PERMISSION`: Location permission required
- `FIRMWARE_PROGRESS`: Firmware update progress

### Device Events
- `DEVICE_CONNECT`: Device connected
- `DEVICE_DISCONNECT`: Device disconnected
- `FEATURES`: Device features updated
- `BUTTON`: Button press detected
- `PIN`: PIN-related events
- `SUPPORT_FEATURES`: Supported feature list

### Firmware Events
- `firmware-release-info`: Firmware update available
- `ble-firmware-release-info`: BLE firmware update available
