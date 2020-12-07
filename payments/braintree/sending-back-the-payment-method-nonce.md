---
description: >-
  How to send the payment method nonce you receive from Braintree back to
  Commerce Layer
---

# Sending back the payment method nonce

## Problem

Your customer has submitted credit card information to Braintree — i.e. through its [Checkout UI](https://www.braintreepayments.com/features/seamless-checkout) integrated into your client. Your client has received a payment method nonce in response and you need to send it back to Commerce Layer to proceed with the payment flow.

{% hint style="info" %}
The term _payment method_ mentioned here when referring to the nonce is used to match [Braintree internal naming convention](https://developers.braintreepayments.com/guides/payment-methods/ruby) and has nothing to do with Commerce Layer API [payment method](https://docs.commercelayer.io/api/resources/payment_methods) concept you'll find elsewhere in the guides.
{% endhint %}

## Solution

To send the payment method nonce back to Commerce Layer you have to update the Braintree payment object. To do that, send a `PATCH` request to the `/api/braintree_payments/:id` endpoint, setting the `payment_method_nonce` attribute.

### Example

{% tabs %}
{% tab title="Request" %}
The following request updates the Braintree payment source identified by the "vdDEAsZYzR" ID with the payment method nonce received from the client:

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
      "payment_method_nonce": "aaaaaa-bbb-ccc-ddd-eeeeee"
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
      "client_token": "xxxx.yyyy.zzzz==",
      "payment_method_nonce": "aaaaaa-bbb-ccc-ddd-eeeeee",
      "options": {},
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/braintree_payments/vdDEAsZYzR/relationships/order",
          "related": "https://yourdomain.commercelayer.io/api/braintree_payments/vdDEAsZYzR/order"
        }
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

See our API reference if you need more information on how to [update a Braintree payment](https://docs.commercelayer.io/api/resources/braintree_payments/update_braintree_payment). See our Checkout guide for more details on how to place an order.

{% page-ref page="../../checkout/placing-the-order.md" %}



