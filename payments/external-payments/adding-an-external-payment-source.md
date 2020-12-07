---
description: How to add an external payment source to an order
---

# Adding an external payment source

## Problem

You have a pending order with a selected payment method that is associated with an external payment gateway. You want to give your customer the possibility to process the payment using the external endpoint you configured.

## Solution

To add an external payment source to an order, you have to create an external payment source object and associate it with the order, as described in the Checkout guide.

{% page-ref page="../../checkout/adding-a-payment-source.md" %}

### Example

#### 1. Get the payment source type

{% tabs %}
{% tab title="Request" %}
The following request retrieves the attributes of the payment method associated with the order identified by the "EqxzYhylVw" ID:

```javascript
curl -X GET \
  http://yourdomain.commercelayer.io/api/orders/EqxzYhylVw?include=payment_method \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated payment method:

```javascript
{
  "data": {
    "id": "EqxzYhylVw",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/EqxzYhylVw"
    },
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
      "available_payment_methods": {
        "links": {...}
      },
      "payment_method": {
	  "links": {...}
	},
        "data": {
          "type": "payment_methods",
          "id": "GmvVesqPmj"
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
      "id": "GmvVesqPmj",
      "type": "payment_methods",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/payment_methods/GmvVesqPmj"
      },
      "attributes": {
        "payment_source_type": "external_payments",
        "name": "External Payment",
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
          "market": {
            "links": {...}
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
The following request creates an external payment object and associates it with the order identified by the "EqxzYhylVw" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/external_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "external_payments",
    "attributes": {
      "payment_source_token": "xxxx.yyyy.zzzz"      
    },
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "EqxzYhylVw"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created external payment object:

```javascript
{
  "data": {
    "id": "xYZkjABcde",
    "type": "external_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/external_payments/xYZkjABcde"
    },
    "attributes": {
      "payment_source_token": "xxxx.yyyy.zzzz",
      "options": null,
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/external_payments/xYZkjABcde/relationships/order",
          "related": "https://yourdomain.commercelayer.io/api/external_payments/xYZkjABcde/order"
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

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create an external payment](https://docs.commercelayer.io/api/resources/external_payments/create_external_payment). See our Checkout guide for more details on how to place an order.

{% page-ref page="../../checkout/placing-the-order.md" %}

