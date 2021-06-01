---
description: How to add a shipping address to the order
---

# Adding a shipping address

## Problem

You need to add a shipping address to a draft order. For example, you've added a billing address to an order but you don't want to use the same address for shipping.

![A sample company shipping address form](../.gitbook/assets/shipping-address-cover%20%284%29.jpg)

## Solution

To add a shipping address to an order, first you need to create the new address and then associate it with the order as its shipping address. To do that:

1. Send a `POST` request to the `/api/addresses` endpoint.
2. Send a `PATCH` request to the `/api/orders/:id` endpoint, setting its `shipping_address` relationship.

### Example

#### 1. Create a new address

{% tabs %}
{% tab title="Request" %}
The following request creates a new address:

```javascript
curl -X POST \
  https://yourdomain.commercelayer.io/api/addresses \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d'{
    "data": {
      "type": "addresses",
      "attributes": {
        "business": true,
        "company": "The Brand SRL",
        "line_1": "123 5th Avenue",
        "city": "New York",
        "zip_code": "10011",
        "state_code": "NY",
        "country_code": "US",
        "phone": "(212) 123-456-7890"
      }
    }
  }'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `201 Created` status code, returning the created address object:

```javascript
{
  "data": {
    "id": "YWoeluJjEB",
    "type": "addresses",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/addresses/YWoeluJjEB"
    },
    "attributes": {
      "business": true,
      "first_name": "null",
      "last_name": "null",
      "company": "The Brand SRL",
      "full_name": "The Brand SRL",
      "line_1": "123 5th Avenue",
      "line_2": null,
      "city": "New York",
      "zip_code": "10011",
      "state_code": "NY",
      "country_code": "US",
      "phone": "(212) 123-456-7890",
      "full_address": "123 5th Avenue, 10011 New York NY (US) (212) 123-456-7890",
      "name": "The Brand SRL, 123 5th Avenue, 10011 New York NY (US) (212) 123-456-7890",
      "email": null,
      "notes": null,
      "lat": null,
      "lng": null,
      "is_localized": false,
      "is_geocoded": false,
      "provider_name": null,
      "map_url": null,
      "static_map_url": null,
      "billing_info": null,
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "metadata": {}
    },
    "relationships": {
      "geocoder": {
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

#### 2. Associate the new address with the order

{% tabs %}
{% tab title="Request" %}
The following request associated the address identified by the "YWoeluJjEB" ID with the order identified by the "NgojhKoyYN" ID:

```javascript
curl -X PATCH \
  https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
      "type": "orders",
      "id": "NgojhKoyYN",
      "relationships": {
        "shipping_address": {
          "data": {
            "type": "addresses",
            "id": "YWoeluJjEB"
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
      "shipping_amount_cents": 0,
      "shipping_amount_float": 0.0,
      "formatted_shipping_amount": "€0,00",
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

The image below shows how each field of a typical shipping address form is mapped to a specific field of the address or order objects.

![A sample company shipping address form field mapping](../.gitbook/assets/new-shipping-address-mapping%20%282%29.jpg)

## Logged customers

When checking out an order as a logged customer — i.e. with a customer access token — you might want to give them the possibility to select a shipping address from their address book instead of creating a new address. 

![A sample address book selection mapping](../.gitbook/assets/select-shipping-address-mapping%20%282%29.jpg)

In this case, first you need to retrieve the list of the customer addresses, and then associate the selected one with the order.

To do that:

1. Send a `GET` request to the `/api/customer_addresses` endpoint and include the associated addresses — the list is automatically filtered by the customer associated with the access token.
2. Send a `PATCH` request to the `/api/orders/:id` endpoint, setting the selected address ID as the `_shipping_address_clone_id`.

{% hint style="warning" %}
Please note that the `_shipping_address_clone_id`  is the ID of the selected address object, **not** the ID of the customer address object.
{% endhint %}

### Example

#### 1. Get the list of customer addresses

{% tabs %}
{% tab title="Request" %}
The following request retrieves the list of customer addresses:

```javascript
curl -X GET \
  https://yourdomain.commercelayer.io/api/customer_addresses?include=address \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Accept: application/vnd.api+json'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the list of customer address objects and the associated addresses:

```javascript
{
  "data": [
    {
      "id": "vGMbRhyyQz",
      "type": "customer_addresses",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/customer_addresses/vGMbRhyyQz"
      },
      "attributes": {
        "name": "The Brand SRL, 123 5th Avenue, 10011 New York NY (US) (212) 123-456-7890",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "customer": {
          "links": {...}
        },
        "address": {
          "links": {...},
          "data": {
            "type": "addresses",
            "id": "YWoeluJjEB"
          }
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "dzeARhvvwz",
      "type": "customer_addresses",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/customer_addresses/dzeARhvvwz"
      },
      "attributes": {
        "name": "John Doe, 456 Steinway Street, 11103 New York NY (US) (347) 321-654-0987",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "customer": {
          "links": {...}
        },
        "address": {
          "links": {...},
          "data": {
            "type": "addresses",
            "id": "BgnguJvXmb"
          }
        }
      },
      "meta": {
        "mode": "test"
      }
    }
  ],
  "included": [
    {
      "id": "YWoeluJjEB",
      "type": "addresses",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/addresses/YWoeluJjEB"
      },
      "attributes": {
        "business": true,
        "first_name": null,
        "last_name": null,
        "company": "The Brand SRL",
        "full_name": "The Brand SRL",
        "line_1": "123 5th Avenue",
        "line_2": null,
        "city": "New York",
        "zip_code": "10011",
        "state_code": "NY",
        "country_code": "US",
        "phone": "(212) 123-456-7890",
        "full_address": "123 5th Avenue, 10011 New York NY (US) (212) 123-456-7890",
        "name": "The Brand SRL, 123 5th Avenue, 10011 New York NY (US) (212) 123-456-7890",
        "email": "touch@example.com",
        "notes": null,
        "lat": null,
        "lng": null,
        "is_localized": false,
        "is_geocoded": false,
        "provider_name": null,
        "map_url": null,
        "static_map_url": null,
        "billing_info": null,
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "geocoder": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "BgnguJvXmb",
      "type": "addresses",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/addresses/BgnguJvXmb"
      },
      "attributes": {
        "business": false,
        "first_name": "John",
        "last_name": "Doe",
        "company": null,
        "full_name": "John Doe",
        "line_1": "456 Steinway Street",
        "line_2": "",
        "city": "New York",
        "zip_code": "11103",
        "state_code": "NY",
        "country_code": "US",
        "phone": "(347) 321-654-0987",
        "full_address": "456 Steinway Street, 11103 New York NY (US) (347) 321-654-0987",
        "name": "John Doe, 456 Steinway Street, 11103 New York NY (US) (347) 321-654-0987",
        "email": null,
        "notes": null,
        "lat": null,
        "lng": null,
        "is_localized": false,
        "is_geocoded": false,
        "provider_name": null,
        "map_url": null,
        "static_map_url": null,
        "billing_info": "",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "geocoder": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    }
  ],
  "meta": {
    "record_count": 2,
    "page_count": 1
  },
  "links": {
    "first": "https://yourdomain.commercelayer.io/api/customer_addresses?include=address&page[number]=1&page[size]=10",
    "last": "https://yourdomain.commercelayer.io/api/customer_addresses?include=address&page[number]=1&page[size]=10"
  }
}
```
{% endtab %}
{% endtabs %}

#### 2. Associate the selected address with the order

{% tabs %}
{% tab title="Request" %}
The following request clones the address identified by the "YWoeluJjEB" ID and associates the newly created address with the order identified by the "NgojhKoyYN" ID as its shipping address:

```javascript
curl -X PATCH \
  https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
      "type": "orders",
      "id": "NgojhKoyYN",
      "attributes": {
        "_shipping_address_clone_id": "YWoeluJjEB"
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
      "guest": false,
      "editable": true,
      "placeable": false,
      "customer_email": "john@example.com",
      "language_code": "en",
      "currency_code": "EUR",
      "tax_included": true,
      "tax_rate": 0.2,
      "freight_taxable": true,
      "requires_billing_info": true,
      "country_code": "IT",
      "shipping_country_code_lock": null,
      "coupon_code": "COUPON12-345",
      "gift_card_code": null,
      "gift_card_or_coupon_code": null,
      "subtotal_amount_cents": 84300,
      "subtotal_amount_float": 843.0,
      "formatted_subtotal_amount": "€843,00",
      "shipping_amount_cents": 0,
      "shipping_amount_float": 0.0,
      "formatted_shipping_amount": "€0,00",
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

## Additional notes

#### Bill to the same address

When adding a shipping address to an order, you may want to use the same address for billing. To do that, send a `PATCH` request to the `/api/orders/:id` endpoint, setting the`_billing_address_same_as_shipping` attribute to `true`.

{% tabs %}
{% tab title="Request" %}
The following request clones the billing address of the order identified by the "NgojhKoyYN" ID and associates the newly created address with the order as its shipping address:

```javascript
curl -X PATCH \
  https://yourdomain.commercelayer.io/api/orders/NgojhKoyYN \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
      "type": "orders",
      "id": "NgojhKoyYN",
      "attributes": {
        "_billing_address_same_as_shipping": true
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
      "tax_rate": 0.2,
      "freight_taxable": true,
      "requires_billing_info": true,
      "country_code": "IT",
      "shipping_country_code_lock": null,
      "coupon_code": "COUPON12-345",
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

## More to read

See our API reference if you need more information on how to [create an address](https://docs.commercelayer.io/api/resources/addresses/create_address), [list customer addresses](https://docs.commercelayer.io/api/resources/customer_addresses/list_customer_addresses), [update an order](https://docs.commercelayer.io/api/resources/orders/update_order), [get a customer token](https://docs.commercelayer.io/api/authentication/password), or [include associations](https://docs.commercelayer.io/api/including-associations).

