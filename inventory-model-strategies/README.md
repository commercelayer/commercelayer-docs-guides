---
description: How to manage the available options in terms of order fulfillment.
---

# Inventory strategies

Commerce Layer supports different [inventory model](https://commercelayer.io/docs/data-model/users-and-organizations) strategies that affect the way the order line items are fulfilled.

Specifically, an inventory model strategy is responsible to decide how many shipments are generated and from which stock location each shipment will be shipped. It is also key in managing the [stock transfers](https://docs.commercelayer.io/api/resources/stock_transfers) \(if any\) from one stock location to another.

![A sample inventory strategy diagram](../.gitbook/assets/inventory-strategies-cover%20%281%29.jpg)

This guide will walk you through the different logic behind each supported strategy. 

{% hint style="info" %}
Apart from the choice of the inventory strategy you want to follow, there's no additional action required from your side. All of the strategies automatically generate the resources based on their predefined logic. Anyway, it can be helpful to know how they work so that you can understand which one best suits your needs.
{% endhint %}

### Supported strategies

These are the inventory model strategies Commerce Layer currently supports. 

{% page-ref page="split-shipments.md" %}

{% page-ref page="transfer-to-primary.md" %}

{% page-ref page="transfer-to-first-available-or-primary.md" %}

{% hint style="info" %}
If no strategy is set on the inventory model the default one — **split shipments** — is used. Otherwise, remember that — in terms of stock location prioritization \(i.e. _primary_ vs _secondary_\) — the smaller the number, the higher the priority.
{% endhint %}

