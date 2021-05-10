---
description: How to add an Checkout.com payment source to an order
---

# Adding the payment source

## Problem

You have a pending order with a selected payment method that is associated with a Checkout.com payment integration. You want to give your customer the possibility to select one of the payment sources available from that gateway — e.g. a credit card — and use it to process the payment.

## Solution

To add a Checkout.com payment source to an order, you have to create a Checkout.com payment source object and associate it with the order, as described in the Checkout guide.

{% page-ref page="../../checkout/adding-a-payment-source.md" %}

### Example

#### 1. Get the payment source type

{% tabs %}
{% tab title="Request" %}
The following request retrieves the attributes of the payment method associated with the order identified by the "eNoKkhzjyN" ID:

```javascript
curl -X GET \
  http://yourdomain.commercelayer.io/api/orders/eNoKkhzjyN?include=payment_method \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated payment method:

```javascript
{
  "data": {
    "id": "eNoKkhzjyN",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/eNoKkhzjyN"
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
          "self": "https://yourdomain.commercelayer.io/api/orders/eNoKkhzjyN/relationships/payment_method",
          "related": "https://yourdomain.commercelayer.io/api/orders/eNoKkhzjyN/payment_method"
        },
      "data": {
        "type": "payment_methods",
        "id": "JMRQlsvxkO"
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
      "id": "JMRQlsvxkO",
      "type": "payment_methods",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/payment_methods/JMRQlsvxkO"
      },
      "attributes": {
        "payment_source_type": "checkout_com_payments",
        "name": "Checkout.com Payment",
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
The following request creates a Checkout.com payment object and associates it with the order identified by the "eNoKkhzjyN" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/checkout_com_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "checkout_com_payments",
    "attributes": {
      "payment_type": "token",
      "token": "tok_ubfj2q76miwundwlk72vxt2i7q"
    },
    "relationships": {
      "order": {
        "data": {
          "type": "orders",
          "id": "eNoKkhzjyN"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created Adyen payment object:

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
      "customer_token": null,
      "redirect_uri": null,
      "payment_response": {},
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

#### Token generation

Checkout.com payments require a token ****that represents the customer's encrypted payment data. The token is generated by **client-side** integration options such as [Frames](https://docs.checkout.com/docs/frames) or [Mobile SDKs](https://docs.checkout.com/docs/sdks#section-mobile-sdk-libraries).

#### Webhooks

Checkout.com payments are asynchronous, which means the payment result it's notified by calling a webhook on Commerce Layer and updating the payment source data accordingly. 

{% hint style="info" %}
Webhooks creation and notification are managed by Commerce Layer and Checkot.com internally, you do not need any configuration on your side.
{% endhint %}

## More to read

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create a Checkout.com payment](https://docs.commercelayer.io/api/resources/checkout_com_payments/create_checkou_com_payment).

