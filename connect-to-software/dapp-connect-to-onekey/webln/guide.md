# Guide

## Overview

This guide helps users understand how to build Bitcoin Lightning Network applications using the OneKey Lightning Network Provider API.



### Building Web Apps **Use Cases**

* Accepting Lightning Network payments
* decentralized identity applications

## Install

#### 1. Download OneKey Wallet

Download the [OneKey Wallet](https://onekey.so/download?client=browserExtension). Install the OneKey browser extension.

#### 2. API Injection Explanation

* The OneKey browser extension injects `Provider API` into visited websites.
* Two types of APIs are provided: `window.webln` and `window.$onekey.webln`.
* It is recommended to use `window.$onekey.webln`.

> Both APIs have the same functionality, just different names.

### Detecting Provider API support

Before you start using WebLN you need to check for browser support by checking if the variable `window.webln` is defined:

```javascript
const provider = (window.$onekey && window.$onekey.webln) || window.webln;

if (!provider) {
  alert("No Provider available.");
}
```

## API

**Clarify Needs**: Identify required functionalities, call relevant interfaces

* Get user's Bitcoin Lightning node information: [`provider.getInfo`](api-reference/getinfo.md)
* Send a payment: [`provider.sendPayment`](api-reference/sendpayment.md)
* Request an invoice to receive payment: [`provider.makeInvoice`](api-reference/makeinvoice.md)
* Request signature for any message: [`provider.signMessage`](api-reference/signmessage.md)

**More Interfaces**: [View Details](../btc-webln/api-reference/)

## Event

* **Key Operations**: Such as account switching
* **More Information**: [View More](event.md)
