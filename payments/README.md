---
description: How to pay for an order using one of the supported payment gateways
---

# Payments

Typically, every payment gateway integration process consists of two main stages: a client-side and a server-side one. 

{% hint style="info" %}
For security and compliance reasons, **Commerce Layer never touches credit card data**.
{% endhint %}

Specifically, the payment details are collected, tokenized — or encrypted — and sent to the payment service on the client-side, leveraging the SDKs and libraries provided by the service itself \(i.e. Commerce Layer always receives a token — never stores, for instance, any credit card information\). On the other hand, our platform takes care of all the server-side part of the process, offering an out-of-the-box integration with the most common payment gateways.

This guide will walk you through the different steps you need to follow in order to let your customers process their payments using the payment method they selected and its corresponding payment gateway or service.

### Supported payment gateways

These are the payment gateways and services Commerce Layer is currently integrated with. Most of them are compliant with the PSD2 European regulation so that you can implement a payment flow that supports SCA and 3DS2. As anticipated, each of them has its own integration pattern. For details and examples of how to implement the payment flow in any of these cases, see the related sections.

{% page-ref page="braintree/" %}

{% page-ref page="stripe/" %}

{% page-ref page="adyen/" %}

{% page-ref page="paypal/" %}

{% page-ref page="manual-payments/" %}

{% page-ref page="external-payments/" %}

If you're planning to use a different service to process your payments, feel free to contact us. We are constantly working on expanding this list and definitely open to adding any additional payment gateway, based on the requests and needs of our clients.

