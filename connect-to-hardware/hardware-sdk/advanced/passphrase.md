# Passphrase

This document aims to provide a comprehensive guide to the Passphrase feature for all users and developers.

***

### Part 1: Core Concepts (For Everyone)

Before diving into technical details, it's crucial to understand what a Passphrase is, how it works, and what your responsibilities are.

#### 1. What is a Passphrase? A Key to a Hidden Space

You can think of a Passphrase as a secret "25th word" that, when combined with your seed phrase, creates an entirely new, independent wallet space.

The basic principle can be summarized by this formula:

* **Standard Wallet** = `Seed Phrase` + `Empty Passphrase`
* **Hidden Wallet A** = `Seed Phrase` + `Passphrase "A"`
* **Hidden Wallet B** = `Seed Phrase` + `Passphrase "B"`

These three wallets are **completely unrelated, and their assets are fully segregated**. This means that even if your seed phrase is compromised, an attacker cannot access your hidden wallets without knowing your Passphrase. They would only see your Standard Wallet.

#### 2. The "Wallet Tree": Why Can One Passphrase Manage Multiple Addresses?

A common question is: "If a Passphrase is a single password, why can I use it to create an account and then create multiple addresses within it?"

The answer is that a Passphrase doesn't create a single address; it creates the root of an entire "wallet tree."

* **The Root (`Seed Phrase + Passphrase`)**: Each unique Passphrase cultivates a unique root. This is the starting point of your hidden wallet's world.
* **The Branches (Accounts)**: From this root, many large branches can grow. These are the "accounts" you create in your wallet.
* **The Leaves (Addresses)**: From each branch, countless leaves can sprout. These are the "addresses" you use to send and receive funds.

So, a Passphrase unlocks an entire hidden tree. The actions you take in the app are simply creating new branches and leaves on that tree.

#### 3. "Re-calculate, Don't Memorize": How Hardware Ensures Security

If a Passphrase is so secret, where is it stored?

**The answer: It is not permanently stored anywhere.**

A hardware wallet is like an advanced calculator with no long-term memory. It doesn't need to "remember" your Passphrase; it "re-calculates" the correct result every single time.

**Here's the workflow:**

1. **When you need to access a hidden wallet**, the app prompts you to enter the Passphrase.
2. The Passphrase you enter is **temporarily** sent to the hardware.
3. The hardware immediately performs a one-time mathematical calculation: `f(its permanently stored seed phrase, the Passphrase you just entered)`.
4. This calculation **deterministically** generates the correct master key for your hidden wallet, which is then used for signing or deriving addresses.
5. After the operation, the hardware **erases** the Passphrase from its temporary memory (RAM). It is also instantly cleared when the device loses power.

This "use-and-forget" mechanism is the cornerstone of Passphrase security, ensuring the most sensitive information exists only for the brief moment it is needed.

#### 4. Your Ultimate Responsibility

* **A Passphrase cannot be recovered**: Neither OneKey nor anyone else can help you retrieve a forgotten Passphrase.
* **It must be perfect**: The slightest difference—a change in case, a space, or a symbol—will lead to a completely different wallet.
* **Loss of Passphrase = Permanent loss of funds**: If you forget the Passphrase, all assets in the corresponding hidden wallet will be **permanently inaccessible**. Please back it up physically and securely.

***

### Part 2: Developer Integration Guide

This section is for developers who need to integrate the OneKey Passphrase feature into their applications.

#### 1. How to Handle Passphrase Requests

When a user enables the Passphrase feature on their hardware, the device enters a state where it expects a Passphrase for operations. If your application makes a request without providing one, the hardware will fire a `UI_REQUEST.REQUEST_PASSPHRASE` event, pausing the execution until your app responds.

There are two main strategies to handle this:

* **Proactive Declaration (Recommended)**: Explicitly state your intention for a specific API call.
* **Reactive Event Handling**: Listen for the event and respond accordingly.

***

#### 2. Proactive Declaration: The `useEmptyPassphrase` Flag

This is the most direct and recommended way to manage Passphrase interactions, especially when you need to **guarantee access to the Standard Wallet**.

If a user has enabled Passphrase on their device, but your application (or a specific feature) needs to access the Standard Wallet (the one without a Passphrase), you can add the `useEmptyPassphrase: true` parameter to your API call.

This flag tells the hardware: "For this specific operation, please use an empty Passphrase." This effectively forces access to the Standard Wallet and **prevents the `REQUEST_PASSPHRASE` event from being triggered.**

**Example: Getting a Bitcoin address from the Standard Wallet**

```javascript
const params = {
  path: "m/84'/0'/0'/0/0",
  coin: "btc",
  // By adding this flag, you ensure access to the Standard Wallet,
  // regardless of the device's Passphrase setting.
  useEmptyPassphrase: true,
};

// This call will now directly target the standard wallet without user interruption.
const response = await HardwareSDK.btcGetAddress(connectId, deviceId, params);
console.log('Standard Wallet Bitcoin Address:', response.payload.address);
```

***

#### 3. Reactive Event Handling: Listening and Responding to Events

If your application needs to support hidden wallets, you must listen for and respond to the `UI_REQUEST.REQUEST_PASSPHRASE` event.

**How to Listen for the Event**

Set up a global listener, preferably during your application's initialization.

```javascript
import { UI_EVENT, UI_REQUEST, UI_RESPONSE } from '@onekeyfe/hd-core';

HardwareSDK.on(UI_EVENT, (message) => {
  if (message.type === UI_REQUEST.REQUEST_PASSPHRASE) {
    // See response scenarios below
    // Example: Let the user enter the Passphrase on the hardware device
    HardwareSDK.uiResponse({
      type: UI_RESPONSE.RECEIVE_PASSPHRASE,
      payload: {
        passphraseOnDevice: true,
        value: '', // Must be empty when passphraseOnDevice is true
      },
    });
  }
});
```

**How to Respond to the Event**

When you receive a `REQUEST_PASSPHRASE` event, you have two primary ways to respond using `HardwareSDK.uiResponse`.

**Scenario 1: Accessing a Hidden Wallet via Hardware Input (Most Secure)**

This is the most secure method. It activates the hardware's own secure input screen, allowing the user to enter the Passphrase directly on the device.

**Implementation:**

```javascript
HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PASSPHRASE,
  payload: {
    // Core: Tell the hardware to handle the input process itself.
    passphraseOnDevice: true,
    // IMPORTANT: In this mode, the 'value' must be an empty string.
    value: '', 
  },
});
```

**Scenario 2: Accessing a Hidden Wallet via Software Input**

This mode allows your application to provide a UI for the user to enter their Passphrase. The entered value is then sent to the hardware.

**Implementation:**

```javascript
const usersInputPassphrase = '...'; // The Passphrase entered by the user in your app

HardwareSDK.uiResponse({
  type: UI_RESPONSE.RECEIVE_PASSPHRASE,
  payload: {
    value: usersInputPassphrase,
    passphraseOnDevice: false,
    // Optional: Request the hardware to cache this Passphrase for the current session.
    save: true, 
  },
});
```

***

#### 4. Session Management with `passphraseState`

Once a hidden wallet is successfully activated (by entering a Passphrase), the SDK provides a `passphraseState` string. This string acts as a temporary session identifier for that specific hidden wallet.

* **Purpose**: To perform subsequent operations on the _same_ hidden wallet without requiring the user to re-enter the Passphrase for every call.
* **Lifecycle**: The `passphraseState` is tied to the hardware's cached Passphrase. If the cache becomes invalid (e.g., device locks, powers off, or cache limit is reached), the `passphraseState` will no longer be valid, and the device will trigger a new `REQUEST_PASSPHRASE` event on the next operation.

**Example Workflow:**

```javascript
async function getBitcoinAddressFromHiddenWallet(connectId, deviceId) {
  // First, get the current passphraseState.
  // If no hidden wallet is active, this may be empty or undefined.
  const passphraseStateRes = await HardwareSDK.getPassphraseState(connectId);

  const params = {
    path: "m/84'/0'/0'/0/0",
    coin: 'btc',
  };

  // If a hidden wallet session is active, include its state.
  if (passphraseStateRes?.payload) {
    params.passphraseState = passphraseStateRes.payload;
  }

  // Make the API call.
  // If the `passphraseState` is valid, this will succeed.
  // If the hardware cache is expired, this call will trigger the `REQUEST_PASSPHRASE`
  // event, which your global listener must handle.
  const response = await HardwareSDK.btcGetAddress(connectId, deviceId, params);

  console.log('Hidden Wallet Bitcoin Address:', response.payload.address);
}
```

_Reference for event handling and session management: `packages/connect-examples/expo-example/src/components/HandleSDKEvents.tsx`_
