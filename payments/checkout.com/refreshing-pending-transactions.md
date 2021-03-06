---
description: How to refresh the status of pending transactions
---

# Refreshing pending transactions

## Problem

You have created a transaction request — [Authorization](https://docs.commercelayer.io/api/resources/authorizations), [Capture](https://docs.commercelayer.io/api/resources/captures), [Void](https://docs.commercelayer.io/api/resources/voids), or [Refund](https://docs.commercelayer.io/api/resources/refunds) — to Checkout.com, which is still pending. You want to refresh its status to check if it gets approved.

## Solution

Checkout.com transactions are asynchronous. That means that once a transaction is created, it remains pending until the Checkout.com webhook updates it. If you want to update the status of the transaction, send a `PATCH` request to the `/api/checkout_com_payments/:id` endpoint, setting the `_refresh` attribute to `true`. 

{% hint style="info" %}
This request will try to refresh any pending transactions and can be used as a fallback strategy in case the Checkout.com webhook isn't fired.
{% endhint %}

### Example

{% tabs %}
{% tab title="Request" %}
The following request updates the Checkout.com payment source identified by the "emdEKhoOMA" ID to refresh any pending transactions:

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
      "_refresh": true
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the updated Checkout.com payment object:

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
      "session_id": "sid_y3oqhf46pyzuxjbcn2giaqnb44",
      "source_id": "src_n5uaa7fr6mwe5nkwmbn244gmhy",
      "customer_token": "cus_pb4u3cqvs36e7p4snxukfhxfrq",
      "redirect_uri": "https://3ds2-sandbox.ckotech.co/interceptor/3ds_plbzo3odshuevkh3bh4tjocl74",
      "payment_response": {
        "3ds": {
            "authentication_response": "Y",
            "cryptogram": "510fc012-ba18-47ba-bf31-0887",
            "downgraded": false,
            "enrolled": "Y",
            "version": "2.1.0",
            "xid": "2e9d7879-d269-4f1f-885e-df4b0a95d79f"
        },
        "_links": {
            "actions": {
                "href": "https://api.sandbox.checkout.com/payments/pay_hce3quuoumtu3cqlgz6vkepjqy/actions"
            },
            "capture": {
                "href": "https://api.sandbox.checkout.com/payments/pay_hce3quuoumtu3cqlgz6vkepjqy/captures"
            },
            "self": {
                "href": "https://api.sandbox.checkout.com/payments/pay_hce3quuoumtu3cqlgz6vkepjqy"
            },
            "void": {
                "href": "https://api.sandbox.checkout.com/payments/pay_hce3quuoumtu3cqlgz6vkepjqy/voids"
            }
        },
        "actions": [
            {
                "id": "act_hce3quuoumtu3cqlgz6vkepjqy",
                "response_code": "10000",
                "response_summary": "Approved",
                "type": "Authorization"
            }
        ],
        "amount": 17,
        "approved": true,
        "billing_descriptor": {
            "city": "London",
            "name": ""
        },
        "currency": "GBP",
        "customer": {
            "id": "cus_6ylx5ay76wou3d4yocniv5qogy"
        },
        "eci": "05",
        "id": "pay_y3oqhf46pyzuxjbcn2giaqnb44",
        "payment_type": "Regular",
        "reference": "ORD-090857",
        "requested_on": "2021-02-15T14:52:39Z",
        "risk": {
            "flagged": false
        },
        "scheme_id": "440925750609151",
        "source": {
            "avs_check": "S",
            "bin": "424242",
            "card_category": "Consumer",
            "card_type": "Credit",
            "cvv_check": "Y",
            "expiry_month": 6,
            "expiry_year": 2028,
            "fast_funds": "d",
            "fingerprint": "35D40AFFDC82BCAC9890181E14655B05D8924C0B4986D29F99D13946A3B59513",
            "id": "src_n5uaa7fr6mwe5nkwmbn244gmhy",
            "issuer": "JPMORGAN CHASE BANK NA",
            "issuer_country": "US",
            "last4": "4242",
            "payouts": true,
            "product_id": "A",
            "product_type": "Visa Traditional",
            "scheme": "Visa",
            "type": "card"
        },
        "status": "Authorized"
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

## More to read

See our API reference if you need more information on how to [update a Checkout.com payment](https://docs.commercelayer.io/api/resources/checkout_com_payments/update_checkout_com_payment). See our Checkout guide for more details on how to place an order.

{% page-ref page="../../checkout/" %}

