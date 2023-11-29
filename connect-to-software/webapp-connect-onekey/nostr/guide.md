# Guide

## Overview

This guide helps users understand how to use the OneKey provider API to easily build network applications using the Nostr protocol.

OneKey provider API is specified by [NIP-07](https://github.com/nostr-protocol/nips/blob/master/07.md).

## Install

#### 1. Download OneKey Wallet

Download the [OneKey Wallet](https://onekey.so/download?client=browserExtension). Install the OneKey browser extension.

#### 2. API Injection Explanation

* The OneKey browser extension injects `Provider API` into visited websites.
* Two types of APIs are provided: `window.nostr` and `window.$onekey.nostr`.
* It is recommended to use `window.$onekey.nostr`.

> Both APIs have the same functionality, just different names.

### Detecting Provider API support

Before you start using Nostr protocol you need to check for browser support by checking if the variable `window.nostr` is defined:

```javascript
const provider = (window.$onekey && window.$onekey.nostr) || window.nostr;

if (!provider) {
  alert("No Provider available.");
}
```

## API

Identify required functionalities, call relevant interfaces

**More Interfaces**: [View Details](api-reference/)

## Event

* **Key Operations**: Such as account switching
* **More Information**: [View More](event.md)
