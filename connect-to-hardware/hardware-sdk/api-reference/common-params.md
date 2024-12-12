# Common Parameters

All SDK method calls accept three common parameters: `connectId`, `deviceId`, and `commonParams`.

```typescript
function methodName(
  connectId: string,
  deviceId: string,
  commonParams: CommonParams
): Promise<Response>
```

## Parameter Details

### Connection Parameters

* `connectId`: string
  - Persistent device connection identifier
  - Obtained from `searchDevice` response
  - Remains constant across device sessions
  - Required for all API calls

* `deviceId`: string
  - Device-specific identifier
  - Obtained from `getFeatures` response
  - Changes when device is reset/wiped
  - Required for all API calls

### Common Parameters Object

```typescript
interface CommonParams {
  // Connection Settings
  retryCount?: number;        // Default: 6
  pollIntervalTime?: number;  // Default: 1000ms
  timeout?: number;           // Connection timeout

  // Session Management
  keepSession?: boolean;      // Persist session after API call
  initSession?: boolean;      // Cache passphraseState

  // Security Features
  passphraseState?: string;   // For passphrase wallet access
  useEmptyPassphrase?: boolean; // Allow empty passphrase

  // Blockchain Specific
  deriveCardano?: boolean;    // Default: true for Cardano methods
}
```

### Usage Examples

1. Basic API Call
```typescript
const response = await HardwareSDK.btcGetAddress(
  device.connectId,
  device.deviceId,
  {
    retryCount: 3,
    timeout: 5000
  }
);
```

2. Passphrase Wallet Access
```typescript
const state = await HardwareSDK.getPassphraseState();
const response = await HardwareSDK.btcGetAddress(
  device.connectId,
  device.deviceId,
  {
    passphraseState: state,
    keepSession: true
  }
);
```

3. Cardano Operation
```typescript
const response = await HardwareSDK.cardanoGetAddress(
  device.connectId,
  device.deviceId,
  {
    deriveCardano: true,
    timeout: 10000  // Longer timeout for Cardano derivation
  }
);
```

### Important Notes

1. Connection Management
   - Increase `retryCount` for unstable connections
   - `pollIntervalTime` increases by 1.5x each retry
   - Set appropriate `timeout` for complex operations

2. Session Handling
   - Use `keepSession` for multiple sequential operations
   - `initSession` caches passphrase state for better UX

3. Blockchain Specific
   - `deriveCardano` affects performance
   - Set to `false` for non-Cardano operations
