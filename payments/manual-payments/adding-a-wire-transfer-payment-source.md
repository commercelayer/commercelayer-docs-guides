---
description: How to add an wire transfer payment source to an order
---

# Adding a wire transfer payment source

## Problem

You have a pending order with a selected payment method that is associated with a manual payment gateway. You want to give your customer the possibility to pay with a wire transfer.

## Solution

To add a wire transfer payment source to an order, you have to create a wire transfer payment source object and associate it with the order, as described in the Checkout guide.

{% page-ref page="../../checkout/adding-a-payment-source.md" %}

### Example

#### 1. Get the payment source type

{% tabs %}
{% tab title="Request" %}
The following request retrieves the attributes of the payment method associated with the order identified by the "yPMWYhmzON" ID:

```javascript
curl -X GET \
  http://yourdomain.commercelayer.io/api/orders/yPMWYhmzON?include=payment_method \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated payment method:

```javascript

  "data": {
    "id": "yPMWYhmzON",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/yPMWYhmzON"
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
      "payment_method": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/orders/yPMWYhmzON/relationships/payment_method",
          "related": "https://yourdomain.commercelayer.io/api/orders/yPMWYhmzON/payment_method"
        },
        "data": {
          "type": "payment_methods",
          "id": "rGEbqRsxMW"
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
      "id": "rGEbqRsxMW",
      "type": "payment_methods",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/payment_methods/rGEbqRsxMW"
      },
      "attributes": {
        "payment_source_type": "wire_transfers",
        "name": "Wire Transfer",
        "disabled_at": null,
        "price_amount_cents": 0,
        "price_amount_float": 0.0,
        "formatted_price_amount": "€0,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
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
The following request creates a wire transfer object and associates it with the order identified by the "yPMWYhmzON" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/wire_transfers \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "wire_transfers",
    "attributes": {},
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "yPMWYhmzON"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created wire transfer object:

```javascript
{
  "data": {
    "id": "ZPOQXtzvGk",
    "type": "wire_transfers",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/wire_transfers/ZPOQXtzvGk"
    },
    "attributes": {
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {
          "self": "https://yourdomain.commercelayer.io/api/wire_transfers/ZPOQXtzvGk/relationships/order",
          "related": "https://yourdomain.commercelayer.io/api/wire_transfers/ZPOQXtzvGk/order"
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

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create a wire transfer](https://docs.commercelayer.io/api/resources/wire_transfers/create_wire_transfer). See our Checkout guide for more details on how to place an order.

{% page-ref page="../../checkout/placing-the-order.md" %}



