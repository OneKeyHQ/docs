# Guide

## Overview

This guide helps users understand how to use the OneKey Provider API to easily build web applications that can interact with BTC.



## Install

#### 1. Download OneKey Wallet

Download the [OneKey Wallet](https://onekey.so/download?client=browserExtension). Install the OneKey browser extension.

#### 2. API Injection Explanation

* The OneKey browser extension injects `Provider API` into visited websites.
* Two types of APIs are provided: `window.unisat` and `window.$onekey.btc`.
* It is recommended to use `window.$onekey.btc`.

> Both APIs have the same functionality, just different names.

### Detecting Provider API support

Before you start using Provider Api you need to check for browser support by checking if the variable `window.unisat` is defined:

```javascript
const provider = (window.$onekey && window.$onekey.btc) || window.unisat;

if (!provider) {
  alert("OneKey Not installed.");
}
```

### Connecting to OneKey

Before using the following API, you first need to use the `provider.requestAccounts` method to request the user to access the relevant BTC account.&#x20;

```
provider.requestAccounts()
```

Other APIs can be called only after user authorization.

## API

**Clarify Needs**: Identify required functionalities, call relevant interfaces

* Get user's account information: [`provider.getAccounts`](api-reference/getaccounts.md)
* Send a payment: [`provider.sendBitcoin`](api-reference/sendbitcoin.md)
* Request signature for any message: [`provider.signMessage`](api-reference/signmessage.md)

**More Interfaces**: [View Details](api-reference/)

## Event

* **Key Operations**: Such as account switching
* **More Information**: [View More](event.md)
