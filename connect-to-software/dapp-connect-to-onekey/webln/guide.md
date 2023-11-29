# Guide

### Overview

This guide helps users understand how to build Bitcoin Lightning Network applications using the OneKey Lightning Network Provider API.

### Building Web Apps **Use Cases**

* Accepting Lightning Network payments
* decentralized identity applications

### API

* **Clarify Needs**: Identify required functionalities, call relevant interfaces
* **Key Interfaces**:
  * Get user's Bitcoin Lightning node information: [`provider.getInfo`](api-reference/getinfo.md)
  * Send a payment: [`provider.sendPayment`](api-reference/sendpayment.md)
  * Request an invoice to receive payment: [`provider.makeInvoice`](api-reference/makeinvoice.md)
  * Request signature for any message: [`provider.signMessage`](api-reference/signmessage.md)
* **More Interfaces**: [View Details](../btc-webln/api-reference/)

### Event

* **Key Operations**: Such as account switching
* **More Information**: [View More](event.md)
