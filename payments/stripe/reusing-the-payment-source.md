---
description: How to save the payment source into the customer wallet
---

# Reusing the payment source

## Problem

You want to save a Stripe payment source already used to complete a purchase into the customer wallet so that it can be reused for future payments.

## Solution

To be able to save a Stripe payment source into the customer wallet, first you must create the payment source properly specifying its options, then you need to update the order accordingly:

1. When creating the payment source, pass the `options` attribute, including the `id` of the payment you've got from [Stripe SDK ](https://stripe.com/docs/api/payment_methods/object)and setting the `setup_future_usage` to `off_session` indicating you're going to store the payment later.
2. To save the created Stripe payment source into the customer wallet you must then send a `PATCH` request to the `/api/orders/:id` endpoint, setting the `_save_payment_source_to_customer_wallet` attribute to `true`.

{% hint style="warning" %}
Remember to take care of the above \(1\) at the time of the payment source **creation**. This way a [Stripe payment intent](https://stripe.com/docs/api/payment_intents) will be properly created. Any further update won't be considered by Stripe and will result in an error.
{% endhint %}

{% hint style="info" %}
To get a list of the customer's available payment sources, include in the request \(2\) the `available_customer_payment_sources`relationship.
{% endhint %}

### Example

#### 1. Create the payment source type

{% tabs %}
{% tab title="Request" %}
The following request creates a reusable Stripe payment object and associates it with the order identified by the "qaMAhZkZvd" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/stripe_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "stripe_payments",
    "attributes": {
      "options": {
        "id": "pm_1IixtoEw0yMev2I0jGzoT9Js",
        "setup_future_usage": "off_session"
      }
    },
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "qaMAhZkZvd"
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
      "stripe_id": "pi_1EywT4IyIXehwXjkgKr5xd6b",
      "options": {
        "id": "pm_1IixtoEw0yMev2I0jGzoT9Js",
        "setup_future_usage": "off_session"
      }
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

#### 2. Save the payment source into the customer wallet

{% tabs %}
{% tab title="Request" %}
The following request save the Stripe payment source to the customer wallet for the order identified by the "qaMAhZkZvd" ID:

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

See our API reference if you need more information on how to [create a Stripe payment source](https://docs.commercelayer.io/api/resources/stripe_payments/create_stripe_payment), [update an order](https://docs.commercelayer.io/api/resources/orders/update_order), or deal with [customer payment sources](https://docs.commercelayer.io/api/resources/customer_payment_sources).

