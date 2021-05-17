---
description: How to save the payment source into the customer wallet
---

# Reusing the payment source

## Problem

You want to save a Checkout.com payment source already used to complete a purchase into the customer wallet so that it can be reused for future payments.

## Solution

To save the payment source into the order's customer wallet you need to ensure that the `payment_response` data are already set, specifically the `source` part identifying the payment. You must then send a `PATCH` request to the `/api/orders/:id` endpoint, setting the `_save_payment_source_to_customer_wallet` attribute to `true`.

{% hint style="info" %}
To get a list of the customer's available payment sources, include in the request the `available_customer_payment_sources`relationship.
{% endhint %}

### Example

{% tabs %}
{% tab title="Request" %}
The following request save the Checkout.com payment source to the customer wallet for the order identified by the "qaMAhZkZvd" ID:

```javascript
curl -X PATCH \
  http://yourdomain.commercelayer.io/api/orders/qaMAhZkZvd?include=available_customer_payment_sources \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "orders",
    "id": "qaMAhZkZvd",
    "attributes": {
      "_save_payment_source_to_customer_wallet": true
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the updated order object:

```javascript
{
  "data": {
    "id": "qaMAhZkZvd",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/qaMAhZkZvd"
    },
    "attributes": {
      ...
    },
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
      "available_customer_payment_sources": {
        "links": {...},
        "data": [
          {
            "type": "customer_payment_sources",
            "id": "QgDXpwsqDx"
          },
          {
            "type": "customer_payment_sources",
            "id": "nbDWkPsgDG"
          }
        ]
      },
      "payment_method": {
        "links": {...}
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
      "id": "QgDXpwsqDx",
      "type": "customer_payment_sources",
      "links": {
          "self": "https://yourdomain.commercelayer.io/api/customer_payment_sources/QgDXpwsqDx"
      },
      "attributes": {
        "customer_token": "1",
        "name": "XXXX-XXXX-XXXX-1111",
        "payment_source_token": "7RxGd2HUyY1Yc2Tghw5N453piFs",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "customer": {
          "links": {...}
        },
        "payment_source": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "nbDWkPsgDG",
      "type": "customer_payment_sources",
      "links": {
          "self": "https://yourdomain.commercelayer.io/api/customer_payment_sources/nbDWkPsgDG"
      },
      "attributes": {
        "customer_token": "1",
        "name": "XXXX-XXXX-XXXX-2222",
        "payment_source_token": "8HxGd2HUyY1Yc2Tghw5N453piFs",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "customer": {
          "links": {...}
        },
        "payment_source": {
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

## More to read

See our API reference if you need more information on how to [update an order](https://docs.commercelayer.io/api/resources/orders/update_order) or deal with [customer payment sources](https://docs.commercelayer.io/api/resources/customer_payment_sources).

