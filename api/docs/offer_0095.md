# Offer (Combo) - 0095

## Publish Offer

SNP publishes offer, in full or incremental catalog refresh:

```json
{
  "context": {
    "action": "on_search",
    "core_version": "1.2.5",
    ..
  },
  "message": {
    "catalog": {
      ..
      "bpp/providers": [
        {
          "id": "P1",
          ..
          "items": [
            {
              "id": "I1",
              ..
            },
            {
              "id": "I2",
              ..
            },
            {
              "id": "I3",
              ..
            }
          ],
          "offers": [
            {
              "id": "combo1",
              "descriptor": {
                "code": "combo",
                "images": [
                  "https://snp.com/images/offer1-banner.webp"
                ]
              },
              "location_ids": [
                "L1"
              ],
              "category_ids": [
                ..
              ],
              "item_ids": [
                "I1",
                "I2"
              ],
              "time": {
                "label": "valid",
                "range": {
                  "start": "2025-01-01T16:00:00.000Z",
                  "end": "2025-01-01T23:00:00.000Z"
                }
              },
              "tags": [
                {
                  "code": "qualifier",
                  "list": [
                    {
                      "code": "item_id",
                      "value": "I4,I5"
                    }
                  ]
                },
                {
                  "code": "benefit",
                  "list": [
                    {
                      "code": "value_type",
                      "value": "percent"
                    },
                    {
                      "code": "value",
                      "value": "-50.00"
                    },
                    {
                      "code": "value_cap",
                      "value": "-200.00"
                    }
                  ]
                },
                {
                  "code": "meta",
                  "list": [
                    {
                      "code": "additive",
                      "value": "no"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    }
                  ]
                }
              ]
            }
          ],
          ..
        }
      ]
    }
  }
}
```

## Apply Offer

Any offer that results in change in the order value and/or quantity of items will be applied to the order quote, and will be a part of all order-level callbacks, i.e., `/on_init`, `/on_confirm`, `/on_status`, `/on_cancel`, `/on_update`:

```json
{
  "context": {
    "action": "on_select",
    "core_version": "1.2.5",
    ..
  },
  "message": {
    "order": {
      ..
      "quote": {
        ..
        "breakup": [
          ..
          {
            "@ondc/org/item_id": "combo1",
            "title": "Flat discount of ₹200 on combo",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "-200.00"
            },
            "item": {
              "tags": [
                {
                  "code": "quote",
                  "list": [
                    {
                      "code": "type",
                      "value": "order"
                    }
                  ]
                },
                {
                  "code": "offer",
                  "list": [
                    {
                      "code": "type",
                      "value": "combo"
                    },
                    {
                      "code": "additive",
                      "value": "no"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    }
                  ]
                }
              ]
            }
          }
        ],
        ..
      }
    }
  }
}
```

## Reverse Offer

Since the "combo" offer resulted in reducing the order value, reversal of the offer will mean adding back the combo discount value, the aggregate of which should not exceed the corresponding combo discount.

For example, the "combo1" offer above reduced the order value by ₹200; this means reversal of the offer can add back, in aggregate, a maximum of ₹200 to the order value:

```json
{
  "context": {
    "action": "on_update",
    "core_version": "1.2.5",
    ..
  },
  "message": {
    "order": {
      "id": "O1",
      ..
      "fulfillments": [
        {
          "id": "F1",
          ..
        },
        {
          "id": "C1",
          ..
          "tags": [
            ..
            {
              "code": "quote_trail",
              "list": [
                {
                  "code": "type",
                  "value": "offer"
                },
                {
                  "code": "id",
                  "value": "combo1"
                },
                {
                  "code": "currency",
                  "value": "INR"
                },
                {
                  "code": "value",
                  "value": "50.00"
                }
              ]
            },
            ..
          ]
        }
      ],
      "quote": {
        ..
        "breakup": [
          ..
          {
            "@ondc/org/item_id": "combo1",
            "title": "Recovery of ₹50 from offered discount",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "-150.00"
            },
            "item": {
              "tags": [
                {
                  "code": "quote",
                  "list": [
                    {
                      "code": "type",
                      "value": "order"
                    }
                  ]
                },
                {
                  "code": "offer",
                  "list": [
                    {
                      "code": "type",
                      "value": "combo"
                    },
                    {
                      "code": "additive",
                      "value": "no"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    }
                  ]
                }
              ]
            }
          }
        ]
      }
    }
  }
}
```
