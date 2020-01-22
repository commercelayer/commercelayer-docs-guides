---
description: How to add new items to your shopping cart
---

# Adding products to cart

## Problem

You want to implement the "add to cart" function on a product page. You have the order ID and either the new item ID or SKU code.

![](../.gitbook/assets/add-to-cart%20%284%29.jpg)

## Solution

Adding a product \(SKU\) to a shopping cart means creating a new line item for an order. To do that, send a `POST` request to the `/api/line_items` endpoint, specifying the order and the SKU relationships.

### Examples

#### Add a SKU to a shopping cart by ID

{% tabs %}
{% tab title="Request" %}
The following request adds the SKU identified by the "xYZkjABcde" ID to the order identified by the "yzkWXfgHQS" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/line_items \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
      "type": "line_items",
      "attributes": {
        "quantity": 2,
        "name": "Grey T-Shirt XL",
        "image_url": "https://img.yourdomain.com/skus/TSHIRTB5B5B5XL.png",
        "_update_quantity": "true"
      },
      "relationships": {
        "order": {
          "data": {
            "type": "orders",
            "id": "yzkWXfgHQS"
          }
        },
        "item": {
          "data": {
            "type": "skus",
            "id": "xYZkjABcde"
          }
        }
      }
    }
  }'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created line item object:

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
      "quantity": 2,
      "currency_code": "EUR",
      "unit_amount_cents": 12900,
      "unit_amount_float": 129,
      "formatted_unit_amount": "129,00€",
      "options_amount_cents": 0,
      "options_amount_float": 0,
      "formatted_options_amount": "0,00€",
      "total_amount_cents": 25800,
      "total_amount_float": 258,
      "formatted_total_amount": "258,00€",
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

If the quantity of the added item results higher than the available one in stock, the API responds with a `422 Unprocessable Entity` error:

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

#### Add a SKU to a shopping cart by code

{% tabs %}
{% tab title="Request" %}
The following request adds the SKU identified by the "TSHIRTB5B5B5XL" code to the order identified by the "yzkWXfgHQS" ID:

```javascript
curl -X POST \
  http://yourdomain.commercelayer.io/api/line_items \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
      "type": "line_items",
      "attributes": {
        "quantity": 2,
        "sku_code": "TSHIRTB5B5B5XL",
        "name": "Grey T-Shirt XL",
        "image_url": "https://img.yourdomain.com/skus/TSHIRTB5B5B5XL.png",
        "_update_quantity": "true"
      },
      "relationships": {
        "order": {
          "data": {
            "type": "orders",
            "id": "yzkWXfgHQS"
          }
        }
      }
    }
  }'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created line item object:

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
      "quantity": 2,
      "currency_code": "EUR",
      "unit_amount_cents": 12900,
      "unit_amount_float": 129,
      "formatted_unit_amount": "129,00€",
      "options_amount_cents": 0,
      "options_amount_float": 0,
      "formatted_options_amount": "0,00€",
      "total_amount_cents": 25800,
      "total_amount_float": 258,
      "formatted_total_amount": "258,00€",
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

If the quantity of the added item results higher than the available one in stock, the API responds with a `422 Unprocessable Entity` error:

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

## Additional notes

#### The `_update_quantity` param

Specifying `"_update_quantity": "true"` in the request body lets you update the existing line item quantity \(if any\) instead of creating a new line item for the same SKU. That means:

* if the item is already present in the shopping cart, its line item quantity attribute is updated
* if the item is not present in the shopping cart a new line item is created and its quantity attribute is set to the `quantity` value

{% hint style="warning" %}
If you send the POST request without the `"_update_quantity": "true"` param a new line item is always created, even if the line item SKU is already present in the shopping cart.
{% endhint %}

#### The `name` and `image_url` fields

When creating a new line item, the only required attribute is `quantity`. Anyway, we recommend to always populate the `name` and `image_url` fields \(like in the examples above\) in order to show the desired name and image in the [cart summary](displaying-the-cart-summary.md) and order management. When empty, Commerce Layer will try to populate `name` and `image_url` from the associated SKU fields.

## More to read

See our API reference if you need more information on how to [create a line item](https://docs.commercelayer.io/api/resources/line_items/create_line_item).

