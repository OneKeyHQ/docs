# signMessage

Sign a message using either ECDSA or BIP322-Simple signing method.

### Method

```typescript
async function signMessage(
  message: string,
  type?: "ecdsa" | "bip322-simple"
): Promise<string>
```

### Params

* `message` — _required_ `string` A string to sign
* `type` — _optional_ `string` Signing method type, either "ecdsa" or "bip322-simple". Default is "ecdsa"

### Wallet & Address Type Support

#### ECDSA Signing
All wallet types (software and hardware) support ECDSA signing for all address types:
- P2PKH (Legacy)
- P2SH (Script Hash)
- P2WPKH (Native Segwit)
- P2TR (Taproot)

#### BIP322-Simple Signing
- Software Wallet:
  - P2WPKH (Native Segwit)
  - P2TR (Taproot)
- Hardware Wallet:
  - Pro and Classic1S models support BIP322-Simple signing

### Returns

`signature` — `string` The signed message signature

### Examples

```typescript
const provider = (window.$onekey && window.$onekey.btc) || window.unisat;

// Sign with ECDSA (default)
try {
  const message = "Hello OneKey";
  const signature = await provider.signMessage(message);
  console.log("ECDSA signature:", signature);
} catch (e) {
  console.error("Error signing with ECDSA:", e);
}

// Sign with BIP322-Simple
try {
  const message = "Hello OneKey";
  const signature = await provider.signMessage(message, "bip322-simple");
  console.log("BIP322-Simple signature:", signature);
} catch (e) {
  console.error("Error signing with BIP322-Simple:", e);
}
```

### Demo

{% embed url="https://codepen.io/OneKeyHQ/pen/OJdojZM" %}
