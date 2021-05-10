---
description: How to add a Stripe payment source to an order
---

# Adding the payment source

## Problem

You have a pending order with a selected payment method that is associated with a Stripe payment integration. You want to give your customer the possibility to select one of the payment sources available from that gateway — e.g. a credit card — and use it to process the payment.

## Solution

To add a Stripe payment source to an order, you have to create a Stripe payment source object and associate it with the order, as described in the Checkout guide.

{% page-ref page="../../checkout/adding-a-payment-source.md" %}

### Example

#### 1. Get the payment source type

{% tabs %}
{% tab title="Request" %}
The following request retrieves the attributes of the payment method associated with the order identified by the "eqkykhjlVw" ID:

```javascript
curl -X GET \
  http://yourdomain.commercelayer.io/api/orders/eqkykhjlVw?include=payment_method \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated payment method:

```javascript
{
  "data": {
    "id": "eqkykhjlVw",
    "type": "orders",
    "links": {...},
    "attributes": {...},
    "relationships": {
      "market": {
        "links": {...}
      },
      "customer": {
        "links": {...}
      },
      "shipping_address": {
        "links": {...}
      },
      "billing_address": {
        "links": {...}
      },
      "available_payment_methods": {
        "links": {...}
      },
      "payment_method": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/orders/eqkykhjlVw/relationships/payment_method",
          "related": "https://yourdomain.commercelayer.io/api/orders/eqkykhjlVw/payment_method"
        },
        "data": {
          "type": "payment_methods",
          "id": "WmOodsVPmQ"
        }
      },
      "payment_source": {
        "links": {...}
      },
      "line_items": {
        "links": {...}
      },
      "shipments": {
        "links": {...} 
      }
    },
    "meta": {
      "mode": "test"
    }
},
"included": [
  {
      "id": "WmOodsVPmQ",
      "type": "payment_methods",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/payment_methods/WmOodsVPmQ"
      },
      "attributes": {
        "payment_source_type": "stripe_payments",
        "name": "Stripe Payment",
        "disabled_at": null,
        "price_amount_cents": 0,
        "price_amount_float": 0.0,
        "formatted_price_amount": "€0,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": "",
        "reference_origin": "",
        "metadata": {}
      },
      "relationships": {
        "market": {
        "links": {...}
        },
        "payment_gateway": {
        "links": {...}
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
The following request creates a Stripe payment object and associates it with the order identified by the "eqkykhjlVw" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/stripe_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "stripe_payments",
    "attributes": {},
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "eqkykhjlVw"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created Stripe payment object:

```javascript
{
  "data": {
    "id": "eqRZMSaNqM",
    "type": "stripe_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/stripe_payments/eqRZMSaNqM"
    },
    "attributes": {
      "client_secret": "xxxx_secret_yyyy",
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
          "self": "https://yourdomain.commercelayer.io/api/stripe_payments/eqRZMSaNqM/relationships/order",
          "related": "https://yourdomain.commercelayer.io/api/stripe_payments/eqRZMSaNqM/order"
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

#### Client secret generation

At the moment of the Stripe payment source creation, a `PaymentIntent` object is created on the server-side — see [Stripe documentation](https://stripe.com/docs/api/payment_intents) for any reference. Its `client_secret` is returned in the related attribute of the response and can be used in your client to securely complete the payment process instead of passing the entire `PaymentIntent` object.

## More to read

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create a Stripe payment](https://docs.commercelayer.io/api/resources/stripe_payments/create_stripe_payment). See our Checkout guide for more details on how to place an order.

{% page-ref page="../../checkout/placing-the-order.md" %}

