---
description: How to select a payment method for the order
---

# Selecting a payment method

## Problem

You have a pending order and you want to give your customer the possibility to select a payment method.

![A sample payment method selection](../.gitbook/assets/select-payment-method-cover%20%282%29.jpg)

## Solution

Within Commerce Layer, an order must have a valid payment method before the order can be placed. To let the customer select a payment method, first you need to get the list of available payment methods for the order and display them to the customer. Then you can associate the selected payment method with the order. To do that:

1.  Send a `GET` request to the `/api/orders/:id` endpoint, including the associated `available_payment_methods`. 
2. Send a PATCH request to the `/api/orders/:id` endpoint, setting its `payment_method` relationship.

### Example

#### 1. Get the available payment methods

{% tabs %}
{% tab title="Request" %}
The following request retrieves the available payment methods associated with the order identified by the "NgojhKoyYN" ID:

```javascript
curl -X GET \ 
  https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN?include=available_payment_methods \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the requested order object and the associated available payment methods:

```javascript
{
  "data": {
    "id": "NgojhKoyYN",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN"
    },
    "attributes": {
      "number": 1234,
      "status": "pending",
      "payment_status": "unpaid",
      "fulfillment_status": "unfulfilled",
      "guest": true,
      "editable": true,
      "placeable": false,
      "customer_email": "john@example.com",
      "language_code": "en",
      "currency_code": "EUR",
      "tax_included": true,
      "tax_rate": null,
      "freight_taxable": null,
      "requires_billing_info": false,
      "country_code": null,
      "shipping_country_code_lock": null,
      "coupon_code": null,
      "gift_card_code": null,
      "gift_card_or_coupon_code": null,
      "subtotal_amount_cents": 84300,
      "subtotal_amount_float": 843.0,
      "formatted_subtotal_amount": "€843,00",
      "shipping_amount_cents": 1000,
      "shipping_amount_float": 10.0,
      "formatted_shipping_amount": "€10,00",
      "payment_method_amount_cents": 0,
      "payment_method_amount_float": 0.0,
      "formatted_payment_method_amount": "€0,00",
      "discount_amount_cents": -8430,
      "discount_amount_float": -84.30,
      "formatted_discount_amount": "-€84,30",
      "adjustment_amount_cents": 0,
      "adjustment_amount_float": 0.0,
      "formatted_adjustment_amount": "€0,00",
      "gift_card_amount_cents": 0,
      "gift_card_amount_float": 0.0,
      "formatted_gift_card_amount": "0,00",
      "total_tax_amount_cents": 16860,
      "total_tax_amount_float": 168.60,
      "formatted_total_tax_amount": "€168,60",
      ...
      "other": "more order attributes",
      ...
      "total_amount_with_taxes_cents": 76870,
      "total_amount_with_taxes_float": 768.70,
      "formatted_total_amount_with_taxes": "€768,70",
      "fees_amount_cents": 0,
      "fees_amount_float": 0.0,
      "formatted_fees_amount": "€0,00",
      "skus_count": 7,
      "line_item_options_count": 0,
      "shipments_count": 0,
      "payment_source_details": null,
      "token": "your-order-token",
      "cart_url": null,
      "return_url": null,
      "terms_url": null,
      "privacy_url": null,
      "checkout_url": "https://checkout.yourdomain.com/NgojhKoyYN",
      "placed_at": null,
      "approved_at": null,
      "cancelled_at": null,
      "payment_updated_at": null,
      "fulfillment_updated_at": null,
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "metadata": {}
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
        "links": {...},
        "data": [
          {
            "type": "payment_methods",
            "id": "zmdjPsavMW"
          },
          {
            "type": "payment_methods",
            "id": "dfGhRwEHkC"
          }
        ]
      },
      "available_customer_payment_sources": {
        "links": {...}
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
      "id": "zmdjPsavMW",
      "type": "payment_methods",
      "links": {
          "self": "https://yourdomain.commercelayer.io/api/payment_methods/zmdjPsavMW"
      },
      "attributes": {
        "payment_source_type": "credit_cards",
        "name": "Credit Card",
        "disabled_at": null,
        "price_amount_cents": 0,
        "price_amount_float": 0.0,
        "formatted_price_amount": "€0,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
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
    },
    {
      "id": "dfGhRwEHkC",
      "type": "payment_methods",
      "links": {
          "self": "https://yourdomain.commercelayer.io/api/payment_methods/dfGhRwEHkC"
      },
      "attributes": {
        "payment_source_type": "paypal_payments",
        "name": "Paypal",
        "disabled_at": null,
        "price_amount_cents": 0,
        "price_amount_float": 0.0,
        "formatted_price_amount": "€0,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
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

#### 2. Select a payment method

{% tabs %}
{% tab title="Request" %}
The following request associated the selected payment method \(identified by the "zmdjPsavMW" ID\) with the order identified by the "NgojhKoyYN" ID:

```javascript
curl -X PATCH \
  https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
      "type": "orders",
      "id": "NgojhKoyYN",
      "relationships": {
        "payment_method": {
          "data": {
            "type": "payment_methods",
            "id": "zmdjPsavMW"
          }
        }
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
    "id": "NgojhKoyYN",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN"
    },
    "attributes": {
      "number": 1234,
      "status": "pending",
      "payment_status": "unpaid",
      "fulfillment_status": "unfulfilled",
      "guest": true,
      "editable": true,
      "placeable": false,
      "customer_email": "john@example.com",
      "language_code": "en",
      "currency_code": "EUR",
      "tax_included": true,
      "tax_rate": 0.22,
      "freight_taxable": true,
      "requires_billing_info": false,
      "country_code": "IT",
      "shipping_country_code_lock": null,
      "coupon_code": "COUPON12-345",
      "gift_card_code": null,
      "gift_card_or_coupon_code": null,
      "subtotal_amount_cents": 84300,
      "subtotal_amount_float": 843.0,
      "formatted_subtotal_amount": "€843,00",
      "shipping_amount_cents": 1000,
      "shipping_amount_float": 10.0,
      "formatted_shipping_amount": "€10,00",
      "payment_method_amount_cents": 0,
      "payment_method_amount_float": 0.0,
      "formatted_payment_method_amount": "€0,00",
      "discount_amount_cents": -8430,
      "discount_amount_float": -84.30,
      "formatted_discount_amount": "-€84,30",
      "adjustment_amount_cents": 0,
      "adjustment_amount_float": 0.0,
      "formatted_adjustment_amount": "€0,00",
      "gift_card_amount_cents": 0,
      "gift_card_amount_float": 0.0,
      "formatted_gift_card_amount": "0,00",
      "total_tax_amount_cents": 16860,
      "total_tax_amount_float": 168.60,
      "formatted_total_tax_amount": "€168,60",
      ...
      "other": "more order attributes",
      ...
      "total_amount_with_taxes_cents": 76870,
      "total_amount_with_taxes_float": 768.70,
      "formatted_total_amount_with_taxes": "€768,70",
      "fees_amount_cents": 0,
      "fees_amount_float": 0.0,
      "formatted_fees_amount": "€0,00",
      "skus_count": 7,
      "line_item_options_count": 0,
      "shipments_count": 0,
      "payment_source_details": null,
      "token": "your-order-token",
      "cart_url": null,
      "return_url": null,
      "terms_url": null,
      "privacy_url": null,
      "checkout_url": "https://checkout.yourdomain.com/NgojhKoyYN",
      "placed_at": null,
      "approved_at": null,
      "cancelled_at": null,
      "payment_updated_at": null,
      "fulfillment_updated_at": null,
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "metadata": {}
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
        "links": {...}
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
  }
}
```
{% endtab %}
{% endtabs %}

### Mapping

The image below shows how the related action during the checkout process is mapped to a specific relationship of the order object.

![A sample payment method selection mapping](../.gitbook/assets/select-payment-method-mapping%20%284%29.jpg)

## Additional notes

#### Redeeming gift cards

When redeeming a gift card, it is possible that the order amount due goes to zero and no additional payment methods are required. Instead, if the gift card balance doesn't cover the total amount of the order, you need to select an additional payment method before it can be placed.

#### Payment source types

Each payment method has a `payment_source_type` attribute which determines the type of payment source that can be used to pay for the order. Many payment source types are strictly related to a specific payment gateway, rather than a payment instrument type. 

For example, if you want to collect credit card payments with Stripe, the payment source type is `stripe_payment`, rather than something like "CreditCard". This lets you leverage the Stripe APIs, official libraries, and documentation to build the integration on the client-side while using Commerce Layer as your server-side endpoint. 

For more details, please refer to the payments guide.

{% page-ref page="../payments/" %}

## More to read

See our API reference if you need more information about the [payment method](https://docs.commercelayer.io/api/resources/payment_methods) and [gift card](https://docs.commercelayer.io/api/resources/gift_cards) objects or on how to [update an order](https://docs.commercelayer.io/api/resources/orders/update_order) and [include associations](https://docs.commercelayer.io/api/including-associations).

