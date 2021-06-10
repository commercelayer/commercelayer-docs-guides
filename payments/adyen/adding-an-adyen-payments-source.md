---
description: How to add an Adyen payment source to an order
---

# Adding the payment source

## Problem

You have a pending order with a selected payment method that is associated with an Adyen payment integration. You want to give your customer the possibility to select one of the payment sources available from that gateway — e.g. a credit card — and use it to process the payment.

## Solution

To add an Adyen payment source to an order, you have to create an Adyen payment source object and associate it with the order, as described in the Checkout guide.

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
        "payment_source_type": "adyen_payments",
        "name": "Adyen Payment",
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
The following request creates an Adyen payment object and associates it with the order identified by the "eNoKkhzjyN" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/adyen_payments \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "adyen_payments",
    "attributes": {},
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
    "type": "adyen_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/adyen_payments/emdEKhoOMA"
    },
    "attributes": {
      "payment_methods": {
        "groups": [
          {
            "name": "Credit Card",
            "types": [
              "visa",
              "mc",
              "amex"
            ]
          }
        ],
        "paymentMethods": [
          {
            "brands": [
              "visa",
              "mc",
              "amex"
            ],
            "details": [
              {
                "key": "encryptedCardNumber",
                "type": "cardToken"
              },
              {
                "key": "encryptedSecurityCode",
                "type": "cardToken"
              },
              {
                "key": "encryptedExpiryMonth",
                "type": "cardToken"
              },
              {
                "key": "encryptedExpiryYear",
                "type": "cardToken"
              },
              {
                "key": "holderName",
                "optional": true,
                "type": "text"
              }
            ],
            "name": "Credit Card",
            "type": "scheme"
          },
          {
            "name": "Online bank transfer.",
            "supportsRecurring": true,
            "type": "directEbanking"
          },
          {
            "name": "Pay later with Klarna.",
            "supportsRecurring": true,
            "type": "klarna"
          },
          {
            "name": "Paysafecard",
            "supportsRecurring": true,
            "type": "paysafecard"
          },
          {
            "details": [
              {
                "key": "bic",
                "type": "text"
              }
            ],
            "name": "GiroPay",
            "supportsRecurring": true,
            "type": "giropay"
          },
          {
            "name": "Slice it with Klarna.",
            "supportsRecurring": true,
            "type": "klarna_account"
          }
        ]
      },
      "payment_request_data": {},
      "payment_request_details": {},
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

#### Available payment methods

At the moment of the Adyen payment source creation, Commerce Layer makes a server-to-server request to Adyen to get a list of the available payment methods based on the payment amount and the customer country/device  — see [Adyen documentation](https://docs.adyen.com/online-payments/drop-in-web#step-1-get-available-payment-methods) for any reference — and returns it in the `payment_methods` attribute of the Ayden payment object.

{% hint style="info" %}
The term _payment method_ mentioned here is used to match [Adyen internal naming convention](https://docs.adyen.com/payment-methods) and has nothing to do with Commerce Layer API [payment method](https://docs.commercelayer.io/api/resources/payment_methods) concept you'll find elsewhere in the guides.
{% endhint %}

## More to read

See our API reference if you need more information on how to [retrieve an order](https://docs.commercelayer.io/api/resources/orders/retrieve_order), [include associations](https://docs.commercelayer.io/api/including-associations), or [create an Adyen payment](https://docs.commercelayer.io/api/resources/adyen_payments/create_adyen_payment).

