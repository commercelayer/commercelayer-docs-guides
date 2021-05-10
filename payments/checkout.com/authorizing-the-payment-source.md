---
description: How to authorize the payment before placing the order
---

# Authorizing the payment source

## Problem

Your order has reached the final step of the payment flow and you need to perform an explicit authorization before placing it.

## Solution

To start the authorization process for the payment, send a `PATCH` request to the `/api/checkout_com_payments/:id` endpoint, setting the `_authorize` attribute to `true`. You can get the additional information needed for the next steps directly in the response.

### Example

{% tabs %}
{% tab title="Request" %}
The following request authorizes the Checkout.com payment source identified by the "emdEKhoOMA" ID:

```javascript
curl -X PATCH \
  http://yourdomain.commercelayer.io/api/checkout_com_payments/emdEKhoOMA \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "checkout_payments",
    "id": "emdEKhoOMA",
    "attributes": {
      "_authorize": true
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the updated Adyen payment object with the payment response information:

```javascript
{
  "data": {
    "id": "emdEKhoOMA",
    "type": "checkout_com_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/checkout_com_payments/emdEKhoOMA"
    },
    "attributes": {
      "payment_type": "token",
      "token": "tok_ubfj2q76miwundwlk72vxt2i7q",
      "session_id": null,
      "source_id": null,
      "customer_token": "cus_pb4u3cqvs36e7p4snxukfhxfrq",
      "redirect_uri": "https://3ds2-sandbox.ckotech.co/interceptor/3ds_plbzo3odshuevkh3bh4tjocl74",
      "payment_response": {
        "3ds": {
          "downgraded": false,
          "enrolled": "Y"
        },
        "_links": {
          "redirect": {
            "href": "https://3ds2-sandbox.ckotech.co/interceptor/3ds_plbzo3odshuevkh3bh4tjocl74"
          },
          "self": {
            "href": "https://api.sandbox.checkout.com/payments/pay_iqemqf6jn3jexkjk4v6owl5pkm"
          }
        },
        "customer": {
          "id": "cus_pb4u3cqvs36e7p4snxukfhxfrq"
        },
        "id": "pay_y3oqhf46pyzuxjbcn2giaqnb44",
        "reference": "Order #1",
        "status": "Pending"
      },
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/adyen_payments/emdEKhoOMA/relationships/order",
          "related": "https://yourdomain.commercelayer.io/api/adyen_payments/emdEKhoOMA/order"
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

## Additional notes

#### 3D Secure authentication

[3DS](https://docs.checkout.com/risk-management/3d-secure) authentication it's enabled by default on Checkout.com transactions. You need to get from the payment source authorization response the `redirect_uri` attribute and use it to redirect your customer. Once your customer has completed the 3DS authentication, the page will [redirect back to your checkout](https://docs.checkout.com/risk-management/3d-secure/3d-secure-2-api-integration) with a `session_id` parameter you must use to properly collect payment details \(see [next section](getting-the-payment-details.md)\).

## More to read

See our API reference if you need more information on how to [update a Checkout.com payment](https://docs.commercelayer.io/api/resources/checkout_com_payments/update_checkout_com_payment).

