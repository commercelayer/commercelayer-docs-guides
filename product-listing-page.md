---
description: How to display prices for a list of products
---

# Product listing page

## Problem

You've created a product listing page \(i.e. category or collection page\) with your favorite CMS and now you want to display the price for each of the products. You have the SKU codes of the products you want to list. 

![A sample product listing page](.gitbook/assets/product-listing-page-cover%20%281%29.jpg)

## Solution

To retrieve the price for each of the SKUs on the page, send a `GET` request to the `/api/skus`endpoint, filter it by code, and include the associated prices.

### Example

{% tabs %}
{% tab title="Request" %}
The following request retrieves the prices of the SKUs identified by a list of specific codes:

```javascript
curl -X GET \
  https://yourdomain.commercelayer.io/api/skus?filter[q][code_in]=TSHIRTG5B5B5BST,TSHIRTWF5F5F5MN,TSHIRTB0A0A0ACL,TSHIRTB212F3FSM&include=prices \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the four SKU objects with the prices included:

```javascript
{
  "data": [
    {
      "id": "WLgbSXqyoZ",
      "type": "skus",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/skus/WLgbSXqyoZ"
      },
      "attributes": {
        "code": "TSHIRTG5B5B5BST",
        "name": "Gray T-Shirt Star",
        "description": "Gray cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
        "image_url": "https://img.yourdomain.com/skus/TSHIRTG5B5B5BST.png",
        "tag_names": "",
        "pieces_per_pack": null,
        "weight": null,
        "unit_of_weight": null,
        "hs_tariff_number": null,
        "do_not_ship": false,
        "do_not_track": false,
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": "TSHIRTG5B5B5B",
        "metadata": {}
      },
      "relationships": {
        "shipping_category": {
          "links": {...}
        },
        "prices": {
          "links": {...},
          "data": [
            {
              "type": "prices",
              "id": "ajKQUMdKQp"
            }
            ]
          },
          "stock_items": {
            "links": {...}
          },
          "delivery_lead_times": {
            "links": {...}
          },
          "sku_options": {
            "links": {...}
          }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
        "id": "ZjwqSNOQKW",
        "type": "skus",
        "links": {
            "self": "https://yourdomain.commercelayer.io/api/skus/ZjwqSNOQKW"
        },
        "attributes": {
            "code": "TSHIRTWF5F5F5MN",
            "name": "White T-Shirt Moon",
            "description": "White cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
            "image_url": "https://img.yourdomain.com/skus/TSHIRTWF5F5F5MN.png",
            "tag_names": "",
            "pieces_per_pack": null,
            "weight": null,
            "unit_of_weight": null,
            "hs_tariff_number": null,
            "do_not_ship": false,
            "do_not_track": false,
            "created_at": "2018-01-01T12:00:00.000Z",
            "updated_at": "2018-01-01T12:00:00.000Z",
            "reference": "TSHIRTWF5F5F5",
            "metadata": {}
        },
        "relationships": {
            "shipping_category": {
                "links": {...}
            },
            "prices": {
                "links": {...},
                "data": [
                    {
                        "type": "prices",
                        "id": "pYwMUWJRzg"
                    }
                ]
            },
            "stock_items": {
                "links": {...}
            },
            "delivery_lead_times": {
                "links": {...}
            },
            "sku_options": {
                "links": {...}
            }
        },
        "meta": {
            "mode": "test"
        }
    },
    {
        "id": "WvAJSyDMqZ",
        "type": "skus",
        "links": {
            "self": "https://yourdomain.commercelayer.io/api/skus/WvAJSyDMqZ"
        },
        "attributes": {
            "code": "TSHIRTB0A0A0ACL",
            "name": "Black T-Shirt Cloud",
            "description": "Black cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
            "image_url": "https://img.yourdomain.com/skus/TSHIRTB0A0A0ACL.png",
            "tag_names": "",
            "pieces_per_pack": null,
            "weight": null,
            "unit_of_weight": null,
            "hs_tariff_number": null,
            "do_not_ship": false,
            "do_not_track": false,
            "created_at": "2018-01-01T12:00:00.000Z",
            "updated_at": "2018-01-01T12:00:00.000Z",
            "reference": "TSHIRTB0A0A0A",
            "metadata": {}
        },
        "relationships": {
            "shipping_category": {
                "links": {...}
            },
            "prices": {
                "links": {...},
                "data": [
                    {
                        "type": "prices",
                        "id": "gQVmUKewQa"
                    }
                ]
            },
            "stock_items": {
                "links": {...}
            },
            "delivery_lead_times": {
                "links": {...}
            },
            "sku_options": {
                "links": {...}
            }
        },
        "meta": {
            "mode": "test"
        }
    },
    {
        "id": "BbpjSNePDZ",
        "type": "skus",
        "links": {
            "self": "https://yourdomain.commercelayer.io/api/skus/BbpjSNePDZ"
        },
        "attributes": {
            "code": "TSHIRTB212F3FSM",
            "name": "Blue T-Shirt Smile",
            "description": "Blue cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
            "image_url": "https://img.yourdomain.com/skus/TSHIRTB212F3FSM.png",
            "tag_names": "",
            "pieces_per_pack": null,
            "weight": null,
            "unit_of_weight": null,
            "hs_tariff_number": null,
            "do_not_ship": false,
            "do_not_track": false,
            "created_at": "2018-01-01T12:00:00.000Z",
            "updated_at": "2018-01-01T12:00:00.000Z",
            "reference": "TSHIRTB212F3F",
            "metadata": {}
        },
        "relationships": {
            "shipping_category": {
                "links": {...}
            },
            "prices": {
                "links": {...},
                "data": [
                    {
                        "type": "prices",
                        "id": "aDPrUElXla"
                    }
                ]
            },
            "stock_items": {
                "links": {...}
            },
            "delivery_lead_times": {
                "links": {...}
            },
            "sku_options": {
                "links": {...}
            }
        },
        "meta": {
            "mode": "test"
        }
    }
  ],
  "included": [
    {
      "id": "ajKQUMdKQp",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/ajKQUMdKQp"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTG5B5B5BST",
        "amount_cents": 12900,
        "amount_float": 129.0,
        "formatted_amount": "€129,00",
        "compare_at_amount_cents": 15000,
        "compare_at_amount_float": 150.0,
        "formatted_compare_at_amount": "€150,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "pYwMUWJRzg",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/pYwMUWJRzg"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTWF5F5F5MN",
        "amount_cents": 11900,
        "amount_float": 119.0,
        "formatted_amount": "€119,00",
        "compare_at_amount_cents": 15000,
        "compare_at_amount_float": 150.0,
        "formatted_compare_at_amount": "€150,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "gQVmUKewQa",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/gQVmUKewQa"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTB0A0A0ACL",
        "amount_cents": 15900,
        "amount_float": 159.0,
        "formatted_amount": "€159,00",
        "compare_at_amount_cents": 19000,
        "compare_at_amount_float": 190.0,
        "formatted_compare_at_amount": "€190,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "aDPrUElXla",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/aDPrUElXla"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTB212F3FSM",
        "amount_cents": 14900,
        "amount_float": 149.0,
        "formatted_amount": "€149,00",
        "compare_at_amount_cents": 17000,
        "compare_at_amount_float": 170.0,
        "formatted_compare_at_amount": "€170,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    }
  ],
  "meta": {
      "record_count": 4,
      "page_count": 1
  },
  "links": {
      "first": "https://yourdomain.commercelayer.io/api/skus?filter[q][code_in]=TSHIRTG5B5B5BST,TSHIRTWF5F5F5MN,TSHIRTB0A0A0ACL,TSHIRTB212F3FSM&include=prices&page[number]=1&page[size]=10",
      "last": "https://yourdomain.commercelayer.io/api/skus?filter[q][code_in]=TSHIRTG5B5B5BST,TSHIRTWF5F5F5MN,TSHIRTB0A0A0ACL,TSHIRTB212F3FSM&include=prices&page[number]=1&page[size]=10"
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This API call doesn't return any detail about the availability of the SKUs because, for performance reasons, the `inventory` information is only returned when fetching a single SKU.
{% endhint %}

### Mapping

The image below shows the two main dynamic elements of the page \(selling price and full price\) and how each of these are mapped to a specific field of the price object.

![A sample product listing page mapping](.gitbook/assets/product-listing-page-mapping.jpg)

## Additional notes

#### Available products and "Add to cart" button

To be _sellable_ in a market, an SKU must have a price in the market's price list and at least one stock item in one of the market's stock locations, regardless of its quantity. This means that out-of-stock items are still considered _sellable_ but they are not _available_ and would return an error when trying to add them to the cart.

If you want to display the _available_ products only and show an "Add to cart" button instead of a "View details" one, we recommend you to refine the above request with an additional filter to make sure that the returned SKUs have an available quantity.

{% tabs %}
{% tab title="Request" %}
The following request retrieves the prices of all the _available_ SKUs:

```javascript
curl -X GET \
  https://yourdomain.commercelayer.io/api/skus?filter[q][code_in]=TSHIRTG5B5B5BST,TSHIRTWF5F5F5MN,TSHIRTB0A0A0ACL,TSHIRTB212F3FSM&filter[q][stock_items_quantity_gt]=0&include=prices \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the SKU objects with the prices included \(please note that the _available_ products are only 3 out of the previous 4\):

```javascript
{
  "data": [
    {
      "id": "WLgbSXqyoZ",
      "type": "skus",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/skus/WLgbSXqyoZ"
      },
      "attributes": {
        "code": "TSHIRTG5B5B5BST",
        "name": "Gray T-Shirt Star",
        "description": "Gray cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
        "image_url": "https://img.yourdomain.com/skus/TSHIRTG5B5B5BST.png",
        "tag_names": "",
        "pieces_per_pack": null,
        "weight": null,
        "unit_of_weight": null,
        "hs_tariff_number": null,
        "do_not_ship": false,
        "do_not_track": false,
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": "TSHIRTG5B5B5B",
        "metadata": {}
      },
      "relationships": {
        "shipping_category": {
          "links": {...}
        },
        "prices": {
          "links": {...},
          "data": [
            {
              "type": "prices",
              "id": "ajKQUMdKQp"
            }
            ]
          },
          "stock_items": {
            "links": {...}
          },
          "delivery_lead_times": {
            "links": {...}
          },
          "sku_options": {
            "links": {...}
          }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
        "id": "ZjwqSNOQKW",
        "type": "skus",
        "links": {
            "self": "https://yourdomain.commercelayer.io/api/skus/ZjwqSNOQKW"
        },
        "attributes": {
            "code": "TSHIRTWF5F5F5MN",
            "name": "White T-Shirt Moon",
            "description": "White cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
            "image_url": "https://img.yourdomain.com/skus/TSHIRTWF5F5F5MN.png",
            "tag_names": "",
            "pieces_per_pack": null,
            "weight": null,
            "unit_of_weight": null,
            "hs_tariff_number": null,
            "do_not_ship": false,
            "do_not_track": false,
            "created_at": "2018-01-01T12:00:00.000Z",
            "updated_at": "2018-01-01T12:00:00.000Z",
            "reference": "TSHIRTWF5F5F5",
            "metadata": {}
        },
        "relationships": {
            "shipping_category": {
                "links": {...}
            },
            "prices": {
                "links": {...},
                "data": [
                    {
                        "type": "prices",
                        "id": "pYwMUWJRzg"
                    }
                ]
            },
            "stock_items": {
                "links": {...}
            },
            "delivery_lead_times": {
                "links": {...}
            },
            "sku_options": {
                "links": {...}
            }
        },
        "meta": {
            "mode": "test"
        }
    },
    {
        "id": "WvAJSyDMqZ",
        "type": "skus",
        "links": {
            "self": "https://yourdomain.commercelayer.io/api/skus/WvAJSyDMqZ"
        },
        "attributes": {
            "code": "TSHIRTB0A0A0ACL",
            "name": "Black T-Shirt Cloud",
            "description": "Black cotton t-shirt. Short sleeves and a round neckline. Unique high quality design.",
            "image_url": "https://img.yourdomain.com/skus/TSHIRTB0A0A0ACL.png",
            "tag_names": "",
            "pieces_per_pack": null,
            "weight": null,
            "unit_of_weight": null,
            "hs_tariff_number": null,
            "do_not_ship": false,
            "do_not_track": false,
            "created_at": "2018-01-01T12:00:00.000Z",
            "updated_at": "2018-01-01T12:00:00.000Z",
            "reference": "TSHIRTB0A0A0A",
            "metadata": {}
        },
        "relationships": {
            "shipping_category": {
                "links": {...}
            },
            "prices": {
                "links": {...},
                "data": [
                    {
                        "type": "prices",
                        "id": "gQVmUKewQa"
                    }
                ]
            },
            "stock_items": {
                "links": {...}
            },
            "delivery_lead_times": {
                "links": {...}
            },
            "sku_options": {
                "links": {...}
            }
        },
        "meta": {
            "mode": "test"
        }
    }
  ],
  "included": [
    {
      "id": "ajKQUMdKQp",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/ajKQUMdKQp"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTG5B5B5BST",
        "amount_cents": 12900,
        "amount_float": 129.0,
        "formatted_amount": "€129,00",
        "compare_at_amount_cents": 15000,
        "compare_at_amount_float": 150.0,
        "formatted_compare_at_amount": "€150,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "pYwMUWJRzg",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/pYwMUWJRzg"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTWF5F5F5MN",
        "amount_cents": 11900,
        "amount_float": 119.0,
        "formatted_amount": "€119,00",
        "compare_at_amount_cents": 15000,
        "compare_at_amount_float": 150.0,
        "formatted_compare_at_amount": "€150,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    },
    {
      "id": "gQVmUKewQa",
      "type": "prices",
      "links": {
        "self": "https://yourdomain.commercelayer.io/api/prices/gQVmUKewQa"
      },
      "attributes": {
        "currency_code": "EUR",
        "sku_code": "TSHIRTB0A0A0ACL",
        "amount_cents": 15900,
        "amount_float": 159.0,
        "formatted_amount": "€159,00",
        "compare_at_amount_cents": 19000,
        "compare_at_amount_float": 190.0,
        "formatted_compare_at_amount": "€190,00",
        "created_at": "2018-01-01T12:00:00.000Z",
        "updated_at": "2018-01-01T12:00:00.000Z",
        "reference": null,
        "metadata": {}
      },
      "relationships": {
        "price_list": {
          "links": {...}
        },
        "sku": {
          "links": {...}
        }
      },
      "meta": {
        "mode": "test"
      }
    }
  ],
  "meta": {
    "record_count": 3,
    "page_count": 1
  },
  "links": {
      "first": "https://spineless.commercelayer.io/api/skus?filter[q][code_in]=TSHIRTG5B5B5BST,TSHIRTWF5F5F5MN,TSHIRTB0A0A0ACL,TSHIRTB212F3FSM&include=prices&page[number]=1&page[size]=10",
      "last": "https://spineless.commercelayer.io/api/skus?filter[q][code_in]=TSHIRTG5B5B5BST,TSHIRTWF5F5F5MN,TSHIRTB0A0A0ACL,TSHIRTB212F3FSM&include=prices&page[number]=1&page[size]=10"
  }
}
```
{% endtab %}
{% endtabs %}

## More to read

See our API reference if you need more information on how to [filter data](https://docs.commercelayer.io/api/filtering-data) and [include associations](https://docs.commercelayer.io/api/including-associations).

