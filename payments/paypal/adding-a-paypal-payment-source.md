---
description: How to add a PayPal payment source to an order
---

# Adding the payment source

## Problem

You have a pending order with a selected payment method that is associated with a PayPal payment integration. You want to give your customer the possibility to select Paypal to process the payment.

## Solution

To add a PayPal payment source to an order, you have to create a PayPal payment source object and associate it with the order, as described in the Checkout guide.

{% page-ref page="../../checkout/adding-a-payment-source.md" %}

### Example

#### 1. Get the payment source type

{% tabs %}
{% tab title="Request" %}
The following request retrieves the attributes of the payment method associated with the order identified by the "jNbQLhMeqo" ID:

```javascript
curl -X GET \
  http://yourdomain.commercelayer.io/api/orders/jNbQLhMeqo?include=payment_method \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated payment method:

```javascript
{
  "data": {
    "id": "jNbQLhMeqo",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/jNbQLhMeqo"
    },
    "attributes": {...},
    "relationships": {
      "market": {
        "links": {...
        }
      },
      "customer": {
        "links": {...
        }
      },
      "shipping_address": {
        "links": {...
        }
      },
      "billing_address": {
        "links": {...
        }
      },
      "payment_method": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/orders/jNbQLhMeqo/relationships/payment_method",
          "related": "https://yourdomain.commercelayer.io/api/orders/jNbQLhMeqo/payment_method"
        },
        "data": {
          "type": "payment_methods",
          "id": "PNMGGzsQMd"
        }
      },
      "payment_source": {
        "links": {...
        }
      },
      "line_items": {
        "links": {...
        }
      },
      "shipments": {
        "links": {...
        }
      },
    },
    "meta": {
      "mode": "test"
    }
  },
  "included": [
    {
      "id": "PNMGGzsQMd",
      "type": "payment_methods",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/payment_methods/PNMGGzsQMd"
      },
      "attributes": {
        "payment_source_type": "paypal_payments",
        "name": "Paypal Payment",
        "disabled_at": null,
        "price_amount_cents": 0,
        "price_amount_float": 0.0,
        "formatted_price_amount": "€0,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": "",
        "reference_origin": null,
        "metadata": {}
      },
      "relationships": {
        "market": {
          "links": {...
          }
        },
        "payment_gateway": {
          "links": {...
          }
        }
      },
      "meta": {
        "mode": "test"
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

#### 2. Create the payment source and associate it with the order

{% tabs %}
{% tab title="Request" %}
The following request creates a PayPal payment object and associates it with the order identified by the "jNbQLhMeqo" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/paypal_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \  
  -d '{
  "data": {
    "type": "paypal_payments",
    "attributes": {
      "return_url": "https://checkout.yourdomain.com/jNbQLhMeqo/paypal",
      "cancel_url": "https://checkout.yourdomain.com/jNbQLhMeqo"
    },
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "jNbQLhMeqo"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created PayPal payment object:

```javascript
{
  "data": {
    "id": "ZaDVpIqzeq",
    "type": "paypal_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/paypal_payments/ZaDVpIqzeq"
    },
    "attributes": {
      "return_url": "https://checkout.yourdomain.com/jNbQLhMeqo/paypal",
      "cancel_url": "https://checkout.yourdomain.com/jNbQLhMeqo",
      "note_to_payer": null,
      "paypal_payer_id": null,
      "name": "paypal_payment #123",
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
          "self": "https://yourdomain.commercelayer.io/api/paypal_payments/ZaDVpIqzeq/relationships/order",
          "related": "https://yourdomain.commercelayer.io/api/paypal_payments/ZaDVpIqzeq/order"
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

The successful PayPal payment creation returns all the pieces of information you need to get the payment approval in the PayPal client-side integration flow, such as the `paypal_id` __and the `approval_url` — see [PayPal documentation](https://developer.paypal.com/docs/integration/direct/payments/paypal-payments/#get-payment-approval) for any reference.

## More to read

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create a PayPal payment](https://docs.commercelayer.io/api/resources/paypal_payments/create_paypal_payment).

