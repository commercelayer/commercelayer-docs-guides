---
description: How to create a new shopping cart (draft order)
---

# Creating a shopping cart

## Problem

You need to create a new shopping cart where the customer can put the items they want to purchase.

## Solution

Within Commerce Layer, a shopping cart is just a draft order, so creating a new shopping cart actually means creating a new order. To do that, all you need to do is to send a `POST` request to the `/api/orders` endpoint.

### Example

{% tabs %}
{% tab title="Request" %}
The following request creates a draft order that acts as a new \(empty\) shopping cart:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/orders \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "orders"
    }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the newly created order object, with its status set to `draft`:

```javascript
{
  "data": {
    "id": "xYZkjABcde",
    "type": "orders",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/orders/xYZkjABcde"
    },
    "attributes": {
      "number": 1234,
      "status": "draft",
      "payment_status": "unpaid",
      "fulfillment_status": "unfulfilled",
      "guest": true,
      "editable": true,
      "placeable": false,
      "customer_email": null,
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
      "subtotal_amount_cents": 0,
      "subtotal_amount_float": 0.0,
      "formatted_subtotal_amount": "€0,00",
      "shipping_amount_cents": 0,
      "shipping_amount_float": 0.0,
      "formatted_shipping_amount": "€0,00",
      "payment_method_amount_cents": 0,
      "payment_method_amount_float": 0.0,
      "formatted_payment_method_amount": "€0,00",
      "discount_amount_cents": 0,
      "discount_amount_float": 0.0,
      "formatted_discount_amount": "€0,00",
      "adjustment_amount_cents": 0,
      "adjustment_amount_float": 0.0,
      "formatted_adjustment_amount": "€0,00",
      "gift_card_amount_cents": 0,
      "gift_card_amount_float": 0.0,
      "formatted_gift_card_amount": "€0,00",
      "total_tax_amount_cents": 0,
      "total_tax_amount_float": 0.0,
      "formatted_total_tax_amount": "€0,00",
      "subtotal_tax_amount_cents": 0,
      "subtotal_tax_amount_float": 0.0,
      "formatted_subtotal_tax_amount": "€0,00",
      "shipping_tax_amount_cents": 0,
      "shipping_tax_amount_float": 0.0,
      "formatted_shipping_tax_amount": "€0,00",
      "payment_method_tax_amount_cents": 0,
      "payment_method_tax_amount_float": 0.0,
      "formatted_payment_method_tax_amount": "€0,00",
      "discount_tax_amount_cents": 0,
      "discount_tax_amount_float": 0.0,
      "formatted_discount_tax_amount": "€0,00",
      "adjustment_tax_amount_cents": 0,
      "adjustment_tax_amount_float": 0.0,
      "formatted_adjustment_tax_amount": "€0,00",
      "total_amount_cents": 0,
      "total_amount_float": 0.0,
      "formatted_total_amount": "€0,00",
      "total_taxable_amount_cents": 0,
      "total_taxable_amount_float": 0.0,
      "formatted_total_taxable_amount": "€0,00",
      "subtotal_taxable_amount_cents": 0,
      "subtotal_taxable_amount_float": 0.0,
      "formatted_subtotal_taxable_amount": "€0,00",
      "shipping_taxable_amount_cents": 0,
      "shipping_taxable_amount_float": 0.0,
      "formatted_shipping_taxable_amount": "€0,00",
      "payment_method_taxable_amount_cents": 0,
      "payment_method_taxable_amount_float": 0.0,
      "formatted_payment_method_taxable_amount": "€0,00",
      "discount_taxable_amount_cents": 0,
      "discount_taxable_amount_float": 0.0,
      "formatted_discount_taxable_amount": "€0,00",
      "adjustment_taxable_amount_cents": 0,
      "adjustment_taxable_amount_float": 0.0,
      "formatted_adjustment_taxable_amount": "€0,00",
      "total_amount_with_taxes_cents": 0,
      "total_amount_with_taxes_float": 0.0,
      "formatted_total_amount_with_taxes": "€0,00",
      "fees_amount_cents": 0,
      "fees_amount_float": 0.0,
      "formatted_fees_amount": "€0,00",
      "skus_count": 0,
      "line_item_options_count": 0,
      "shipments_count": 0,
      "payment_source_details": null,
      "token": "your-order-token",
      "cart_url": null,
      "return_url": null,
      "terms_url": null,
      "privacy_url": null,
      "checkout_url": "https://checkout.yourdomain.com/xYZkjABcde",
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

## Additional notes

#### Market scope

The new order will be automatically associated to the market embedded in the request access token, as its scope.

#### Empty carts

For performance reasons, we strongly recommend to avoid dealing with empty carts and to create a new order right before adding the first line item to it.

## More to read

See our API reference if you need more information on how to [create an order](https://docs.commercelayer.io/api/resources/orders/create_order).

