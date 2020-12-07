---
description: How to add a Braintree payment source to an order
---

# Adding a Braintree payment source

## Problem

You have a pending order with a selected payment method that is associated with a Braintree payment integration. You want to give your customer the possibility to select one of the payment sources available from that gateway — e.g. a credit card — and use it to process the payment.

## Solution

To add a Braintree payment source to an order, you have to create a Braintree payment source object and associate it with the order, as described in the Checkout guide.

{% page-ref page="../../checkout/adding-a-payment-source.md" %}

### Example

#### 1. Get the payment source type

{% tabs %}
{% tab title="Request" %}
The following request retrieves the attributes of the payment method associated with the order identified by the "BwAezhyOQw" ID:

```javascript
curl -X GET \
  http://yourdomain.commercelayer.io/api/orders/BwAezhyOQw?include=payment_method \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated payment method:

```javascript
{
  "data": {
    "id": "BwAezhyOQw",
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
        "links": {...},
        "data": {
          "type": "payment_methods",
          "id": "QElGasOdMl"
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
      "id": "QElGasOdMl",
      "type": "payment_methods",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/payment_methods/QElGasOdMl"
      },
      "attributes": {
        "payment_source_type": "braintree_payments",
        "name": "Braintree Payment",
        "disabled_at": null,
        "price_amount_cents": 0,
        "price_amount_float": 0.0,
        "formatted_price_amount": "$0.00",
        "created_at": "2019-07-23T07:45:14.189Z",
        "updated_at": "2019-07-23T07:45:14.189Z",
        "reference": "",
        "reference_origin": null,
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
    "attributes": {},
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

## Additional notes

#### Client token generation

At the moment of the Braintree payment source creation, Commerce Layer generates a client token using your Braintree account API credentials — see [Braintree documentation](https://developers.braintreepayments.com/guides/authorization/client-token) for any reference — and returns it in the `client_token` attribute of the Braintree payment object. It contains all the authorization and configuration information your client needs to initialize the [Braintree SDK](https://developers.braintreepayments.com/guides/client-sdk/setup/javascript/v3). 

## More to read

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create a Braintree payment](https://docs.commercelayer.io/api/resources/braintree_payments/create_braintree_payment).

