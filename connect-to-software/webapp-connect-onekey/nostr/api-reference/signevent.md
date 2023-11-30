# signEvent

### Method

```
async function signEvent(event: UnsignedEvent): Event 
```

### Params

* `event` — _required_ `object`
  * `kind` — _required_ `number`
  * `content`— _required_ `string`
  * `tags` — _required_ `Array<Array<string>>`
  * `created_at` — _required_ `number`

```typescript
export type UnsignedEvent = {
  kind: EventKind;
  content: string;
  tags: string[][];
  created_at: number;
};

export enum EventKind {
  Metadata = 0,
  Text = 1,
  RelayRec = 2,
  Contacts = 3,
  DM = 4,
  Deleted = 5,
}
```

### Response

takes an event object, adds `id`, `pubkey` and `sig` and returns it

### Result

```typescript
export type Event = {
  id: string;
  kind: EventKind;
  pubkey: string;
  content: string;
  tags: string[][];
  created_at: number;
  sig: string;
};
```

### Example

```typescript
const event = {
  created_at: 1700000000,
  kind: 1,
  content: "OneKey 🚀",
  tags: [],
};
const provider = (window.$onekey && window.$onekey.nostr) || window.nostr;

const signedEvent = await provider.signEvent(event);
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/YzBOVgV" %}
