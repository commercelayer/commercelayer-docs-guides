---
description: How to support Braintree local payments
---

# Accepting local payments

## Problem

You want to allow your customers to checkout with the Braintree-supported [local payment methods](https://developers.braintreepayments.com/guides/local-payment-methods/overview). You've properly [configured](https://developers.braintreepayments.com/guides/local-payment-methods/configuration/javascript/v3) your account in the Braintree control panel and integrated their client [SDK](https://developers.braintreepayments.com/guides/local-payment-methods/client-side-custom/javascript/v3). You now need to set up the payment source in order to support the different local payment workflows.

## Solution

The first steps to setup local payments are similar to the standard ones, with the only notable difference you need to set the `local`  attribute as `true` when you create the payment source.

{% page-ref page="braintree/adding-a-braintree-payment-source.md" %}

After that, paths differ based on two possible scenarios — whether the customer is [returning to your checkout](accepting-local-payments.md#customer-returning-to-your-checkout) or [not](accepting-local-payments.md#customers-leaving-your-site-after-payment).

### Customers returning to your checkout

Assuming the customer goes back to your checkout application, you need to update the payment source with the `payment_method_nonce` returned by Braintree as with standard payments. 

{% page-ref page="braintree/sending-back-the-payment-method-nonce.md" %}

Then you have just to update the order with the `_place` attribute in order to complete the payment. 

{% page-ref page="../checkout/placing-the-order.md" %}

{% hint style="info" %}
Upon successful response by Braintree, the order will be captured and approved automatically, since the transaction is already settled by the local payment gateway chosen by the customer.
{% endhint %}

### Customers leaving your site after payment

If the customer doesn't return to your checkout application, you need to follow a slightly different workflow:

1. Configure [Braintree webhooks](https://developers.braintreepayments.com/guides/webhooks/overview) to notify Commerce Layer about the payment results — to do that, use the `webhook_endpoint_url` exposed by the Braintree gateway resource.
2. Update it with the `payment_id` returned by the client [SDK](https://developers.braintreepayments.com/guides/local-payment-methods/client-side-custom/javascript/v3#start-the-payment), in order to let our webhook find and update the payment source used for the payment.
3. Monitor the order's status, since our webhook will update the payment source with the `payment_method_nonce`, place the order, and — upon successful response by Braintree — capture and approve it in a single step.

### Example

#### 1. Create the local payment source

{% tabs %}
{% tab title="Request" %}
The following request creates a Braintree payment object and associates it with the order identified by the "BwAezhyOQw" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/braintree_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "braintree_payments",
    "attributes": {
      "local": true
    },
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "BwAezhyOQw"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created Braintree payment object:

```javascript
{
  "data": {
    "id": "vdDEAsZYzR",
    "type": "braintree_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/braintree_payments/vdDEAsZYzR"
    },
    "attributes": {
      "client_token": "xxxx.yyyy.zzzz",
      "payment_method_nonce": null,
      "payment_id": null,
      "local": true,
      "options": {},
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {...}
      }
    },
    "meta": {
      "mode": "test"
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### 2. Update the payment source with the payment ID

{% tabs %}
{% tab title="Request" %}
The following request updates the Braintree payment source identified by the "vdDEAsZYzR" ID with the `payment_id` received from the client:

```javascript
curl -X PATCH \
  http://yourdomain.commercelayer.io/api/braintree_payments/vdDEAsZYzR \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "braintree_payments",
    "id": "vdDEAsZYzR",
    "attributes": {
      "payment_id": "xxxx.yyyy.zzzz"
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the updated Braintree payment object:

```javascript
{
  "data": {
    "id": "vdDEAsZYzR",
    "type": "braintree_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/braintree_payments/vdDEAsZYzR"
    },
    "attributes": {
      "client_token": "xxxx.yyyy.zzzz",
      "payment_method_nonce": null,
      "payment_id": "xxxx.yyyy.zzzz",
      "local": true,
      "options": {},
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {...}
      }
    },
    "meta": {
      "mode": "test"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## More to read

See our API reference if you need more information on how to [create](https://docs.commercelayer.io/api/resources/braintree_payments/create_braintree_payment) and [update a Braintree payment](https://docs.commercelayer.io/api/resources/braintree_payments/update_braintree_payment) or if you need to learn more about [Braintree gateways](https://docs.commercelayer.io/api/resources/braintree_gateways).

