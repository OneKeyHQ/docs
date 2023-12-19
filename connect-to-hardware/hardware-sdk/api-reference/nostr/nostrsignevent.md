# nostrSignEvent

### Use requirement

* Firmware version required
  * Touch: 4.6.0
  * Classic/Mini: 3.5.0
  * Classic1s: 3.6.0

## Nostr: Sign nostr event

Asks device to sign given nostr event using the private key derived by given BIP32 path. User is asked to confirm all transaction details on OneKey.

```typescript
const result = await HardwareSDK.nostrSignEvent(connectId, deviceId, params);
```

### Params

[**Optional common params**](../common-params.md)

* `path` — _required_ `string | Array<number>` minimum length is `3`. [read more](../path.md)
* `event` — _required_ `NostrEvent` nostr event.

```
enum EventKind {
  Metadata = 0,
  Text = 1,
  RelayRec = 2,
  Contacts = 3,
  DM = 4,
  Deleted = 5,
  Reaction = 7,
  BadgeAward = 8,
  ChannelCreation = 40,
  ChannelMetadata = 41,
  ChannelMessage = 42,
  ChannelHideMessage = 43,
  Reporting = 1984,
  ZapRequest = 9734,
  Zap = 9735,
  RelayListMetadata = 10002,
  ClientAuthentication = 22242,
  NostrConnect = 24133,
  ProfileBadges = 30008,
  BadgeDefinition = 30009,
  LongFormContent = 30023,
  ApplicationSpecificData = 30078,
}

type NostrEvent = {
  id?: string;
  kind: EventKind;
  pubkey?: string;
  content: string;
  tags: string[][];
  created_at: number;
  sig?: string;
};
```



### Examples

```typescript
const response = await HardwareSDK.nostrSignEvent(
  connectId,
  deviceId,
  {
    path: string,
    event: {
      kind: 1,
      content: 'Hello world',
      tags: [],
      created_at: 1702268010,
    },
  }
);
```

Result

```typescript
{
  success: true,
  payload: {
    event: {
      kind: 1,
      content: 'Hello world',
      tags: [],
      created_at: 1702268010,
      sig: '',
    },
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
