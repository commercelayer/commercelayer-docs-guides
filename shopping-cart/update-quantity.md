---
description: How to update the quantity of an item in your shopping cart
---

# Updating cart quantities

## Problem

You have some items in your shopping cart and you want to update their quantity.

![](../.gitbook/assets/cart-summary-zoom-update-quantity-3-lines%20%281%29.jpg)

## Solution

To set the quantity of an item in your shopping cart you need to change its `quantity` attribute. To do that, send a `PATCH` request to the `/api/line_items/{{line_item_id}}` endpoint, where `{{line_item_id}}` is the ID of the line item that you want to update. 

### Example

{% tabs %}
{% tab title="Request" %}
The following request updates the line item identified by the "aBmNkPQRst" ID with a quantity of 6:

```javascript
curl -X PATCH \
  http://yourdomain.commercelayer.io/api/line_items/aBmNkPQRst \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -d '{
  "data": {
    "id": "aBmNkPQRst",
    "type": "line_items",
    "attributes": {
      "quantity": 6
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the line item object updated with the new quantity:

```javascript
{
  "data": {
    "id": "aBmNkPQRst",
    "type": "line_items",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/line_items/aBmNkPQRst"
    },
    "attributes": {
      "sku_code": "TSHIRTB5B5B5XL",
      "quantity": 6,
      "currency_code": "EUR",
      "unit_amount_cents": 12900,
      "unit_amount_float": 129,
      "formatted_unit_amount": "129,00€",
      "options_amount_cents": 0,
      "options_amount_float": 0,
      "formatted_options_amount": "$0.00",
      "total_amount_cents": 77400,
      "total_amount_float": 774,00,
      "formatted_total_amount": "774,00€",
      "name": "Grey T-Shirt XL",
      "image_url": "https://img.yourdomain.com/skus/TSHIRTB5B5B5XL.png",
      "tax_rate": null,
      "tax_breakdown": {},
      "item_type": "skus",
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {...}
      },
      "item": {
        "links": {...}
      },
      "line_item_options": {
        "links": {...}
      }
    },
    "meta": {
      "mode": "test"
    }
  }
}
```

If the updated quantity results higher than the available quantity in stock, the API responds with a `422 Unprocessable Entity` error:

```javascript
{
  "errors": [
    {
      "title": "is out of stock",
      "detail": "quantity - is out of stock",
      "code": "VALIDATION_ERROR",
      "source": {
        "pointer": "/data/attributes/quantity"
      },
      "status": "422",
      "meta": {
        "error": "out_of_stock"
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## More to read

See our API reference if you need more information on how to [update a line item](https://docs.commercelayer.io/api/resources/line_items/update_line_item).

