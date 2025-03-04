## Offer (Discount) - 0091

### Publish Offer
SNP publishes the offer in full or incremental catalog refresh:

```json
{
  "context": {
    "action": "on_search",
    "core_version": "1.2.5",
    ...
  },
  "message": {
    "catalog": {
      ...
      "bpp/providers": [
        {
          "id": "P1",
          ...
          "items": [
            {
              "id": "I1",
              ...
            },
            {
              "id": "I2",
              ...
            },
            {
              "id": "I3",
              ...
            }
          ],
          "offers": [
            {
              "id": "discp60",
              "descriptor": {
                "code": "discount",
                "images": [
                  "https://snp.com/images/offer1-banner.webp"
                ]
              },
              "location_ids": [
                "L1"
              ],
              "category_ids": [
                ...
              ],
              "item_ids": [
                "I1"
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
                      "code": "min_value",
                      "value": "159.00"
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
                      "value": "-60.00"
                    },
                    {
                      "code": "value_cap",
                      "value": "-120.00"
                    }
                  ]
                },
                {
                  "code": "meta",
                  "list": [
                    {
                      "code": "additive",
                      "value": "yes"
                    },
                    {
                      "code": "auto",
                      "value": "no"
                    }
                  ]
                }
              ]
            },
            {
              "id": "flat150",
              "descriptor": {
                "code": "discount",
                "images": [
                  "https://snp.com/images/offer2-banner.webp"
                ]
              },
              "location_ids": [
                "L1"
              ],
              "category_ids": [
                "C1",
                "C2"
              ],
              "item_ids": [
                "I1"
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
                      "code": "min_value",
                      "value": "499.00"
                    }
                  ]
                },
                {
                  "code": "benefit",
                  "list": [
                    {
                      "code": "value_type",
                      "value": "amount"
                    },
                    {
                      "code": "value",
                      "value": "-150.00"
                    }
                  ]
                },
                {
                  "code": "meta",
                  "list": [
                    {
                      "code": "additive",
                      "value": "yes"
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
          ...
        }
      ]
    }
  }
}
```

---

### Apply Offer
Any offer that results in a change in the order value and/or quantity of items will be applied to the order quote and will be part of all order-level callbacks, i.e., `/on_init`, `/on_confirm`, `/on_status`, `/on_cancel`, `/on_update`:

```json
{
  "context": {
    "action": "select",
    "core_version": "1.2.5",
    ...
  },
  "message": {
    "order": {
      ...
      "offers": [
        {
          "id": "discp60"
        }
      ],
      ...
    }
  }
}
```

```json
{
  "context": {
    "action": "on_select",
    "core_version": "1.2.5",
    ...
  },
  "message": {
    "order": {
      ...
      "quote": {
        ...
        "breakup": [
          ...
          {
            "@ondc/org/item_id": "flat150",
            "title": "Flat ₹150 off on ₹499 minimum cart value",
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
                      "value": "discount"
                    },
                    {
                      "code": "additive",
                      "value": "yes"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    }
                  ]
                }
              ]
            }
          },
          {
            "@ondc/org/item_id": "discp60",
            "title": "₹120 off on minimum cart value of ₹159",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "-120.00"
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
                      "value": "discount"
                    },
                    {
                      "code": "additive",
                      "value": "yes"
                    },
                    {
                      "code": "auto",
                      "value": "no"
                    }
                  ]
                }
              ]
            }
          }
        ],
        ...
      }
    }
  }
}
```

```json
{
  "context": {
    "action": "init",
    "core_version": "1.2.5",
    ...
  },
  "message": {
    "order": {
      ...
      "offers": [
        {
          "id": "flat150",
          "tags": [
            {
              "code": "selection",
              "list": [
                {
                  "code": "apply",
                  "value": "no"
                }
              ]
            }
          ]
        }
      ],
      ...
    }
  }
}
```

---

### Reverse Offer
Since the "discount" offer resulted in monetary benefit to the buyer (by reducing the order value), reversal of the offer will mean reversing this, up to the benefit provided to the buyer in aggregate.

For example, the "discp60" offer above reduced the order value by ₹120; this means reversal of the offer can add back, in aggregate, a maximum of ₹120 to the order value.

```json
{
  "context": {
    "action": "on_update",
    "core_version": "1.2.5",
    ...
  },
  "message": {
    "order": {
      "id": "O1",
      ...
      "fulfillments": [
        {
          "id": "F1",
          ...
        },
        {
          "id": "C1",
          ...
          "tags": [
            ...
            {
              "code": "quote_trail",
              "list": [
                {
                  "code": "type",
                  "value": "offer"
                },
                {
                  "code": "id",
                  "value": "discp60"
                },
                {
                  "code": "currency",
                  "value": "INR"
                },
                {
                  "code": "value",
                  "value": "60.00"
                }
              ]
            },
            ...
          ]
        }
      ],
      "quote": {
        ...
        "breakup": [
          ...
          {
            "@ondc/org/item_id": "discp60",
            "title": "rolling back ₹60 from offered discount",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "-60.00"
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
                      "value": "discount"
                    },
                    {
                      "code": "additive",
                      "value": "yes"
                    },
                    {
                      "code": "auto",
                      "value": "no"
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