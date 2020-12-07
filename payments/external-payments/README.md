---
description: How to pay for an order using an external payment gateway
---

# External payments

Commerce Layer is integrated with some of the most popular payment gateways. Adding new payment gateways to our core is already provided for in our roadmap. Anyway, you already have the complete flexibility to connect whichever payment service provider you may need. 

To do that, just create an external payment gateway and define your custom endpoint\(s\) responding to the **Authorization**, **Capture**, **Void**, and **Refund** transaction types, as described in the [External Payments](https://docs.commercelayer.io/api/external-resources/external-payment-gateways) section of our API reference. Your external endpoint will be responsible for the actual integration with the payment gateway. 

This section will walk you through the steps you need to follow in order to let your customers process their payments using an external gateway on our platform.

