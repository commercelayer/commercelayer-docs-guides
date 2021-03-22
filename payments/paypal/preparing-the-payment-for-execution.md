---
description: How to make a payment ready to be executed on Paypal
---

# Preparing the payment for execution

## Problem

Your customer has approved the payment on Paypal – i.e. through [PayPal checkout](https://developer.paypal.com/docs/checkout/) — and has been redirected to your client. You need to make sure it's ready to be executed.

## Solution

To enable the payment execution with PayPal, you need to update the PayPal payment object. To do that, send a `PATCH` request to the `/api/paypal_payments/:id` endpoint, filling the `payment_payer_id` with the payer ID you just received from the client – see [PayPal documentation](https://developer.paypal.com/docs/integration/direct/payments/paypal-payments/#execute-payment) for any reference.

### Example

{% tabs %}
{% tab title="Request" %}
The following request updates the PayPal payment source identified by the "qaZPVIyoeg" ID with the payer ID received from the client:

```javascript
curl -X PATCH \
  http://yourdomain.commercelayer.io/api/paypal_payments/qaZPVIyoeg \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "paypal_payments",
    "id": "qaZPVIyoeg",
    "attributes": {
      "paypal_payer_id": "aaabbbccc123"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the updated PayPal payment object:

```javascript
{
  "data": {
    "id": "qaZPVIyoeg",
    "type": "paypal_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/paypal_payments/qaZPVIyoeg"
    },
    "attributes": {
      "return_url": "https://checkout.yourdomain.com/jNbQLhMeqo/paypal",
      "cancel_url": "https://checkout.yourdomain.com/jNbQLhMeqo",
      "note_to_payer": null,
      "paypal_payer_id": "aaabbbccc123",
      "name": "aaabbbccc123",
      "paypal_id": "PAYID-xxxxyyyyzzzz",
      "status": "created",
      "approval_url": "https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&token=EC-123abc456",
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {
          "self": "https://commerce-layer.commercelayer.co/api/paypal_payments/qaZPVIyoeg/relationships/order",
          "related": "https://commerce-layer.commercelayer.co/api/paypal_payments/qaZPVIyoeg/order"
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

See our API reference if you need more information on how to [update a PayPal payment](https://docs.commercelayer.io/api/resources/paypal_payments/update_paypal_payment). See our Checkout guide for more details on how to place an order.

{% page-ref page="../../checkout/placing-the-order.md" %}





