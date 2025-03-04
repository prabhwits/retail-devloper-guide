## Offer (BuyXGetY) - 0092

### Publish Offer
SNP publishes the offer in full or incremental catalog refresh:

#### Scenario 1 - Offer for Specific Item(s) in Cart

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
              "id": "buy2get3",
              "descriptor": {
                "code": "buyXgetY",
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
                      "code": "item_count",
                      "value": "2"
                    }
                  ]
                },
                {
                  "code": "benefit",
                  "list": [
                    {
                      "code": "item_count",
                      "value": "1"
                    },
                    {
                      "code": "item_id",
                      "value": "I2"
                    },
                    {
                      "code": "item_value",
                      "value": "0.00"
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
          ...
        }
      ]
    }
  }
}
```

---

### Apply Offer

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
      "items": [
        {
          "id": "I2",
          ...
          "quantity": {
            "count": 2
          }
        }
      ],
      ...
    }
  }
}
```

Any offer that results in a change in the order value and/or quantity of items will be applied to the order quote and will be part of all order-level callbacks, i.e., `/on_init`, `/on_confirm`, `/on_status`, `/on_cancel`, `/on_update`:

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
          {},
          {
            "@ondc/org/item_id": "buy2get3",
            "@ondc/org/item_quantity": {
              "count": 1
            },
            "title": "buy 2 items, get 3rd for free or at offered price",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "0.00"
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
                      "value": "buyXgetY"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    },
                    {
                      "code": "additive",
                      "value": "no"
                    },
                    {
                      "code": "item_id",
                      "value": "I2"
                    },
                    {
                      "code": "item_count",
                      "value": "1"
                    },
                    {
                      "code": "item_value",
                      "value": "0.00"
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

---

#### Scenario 2 - Offer for Any Item(s) in Cart

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
              "id": "buy2get3",
              "descriptor": {
                "code": "buyXgetY",
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
              "item_ids": [],
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
                      "code": "item_count",
                      "value": "2"
                    }
                  ]
                },
                {
                  "code": "benefit",
                  "list": [
                    {
                      "code": "item_count",
                      "value": "1"
                    },
                    {
                      "code": "item_id",
                      "value": "I1"
                    },
                    {
                      "code": "item_value",
                      "value": "0.00"
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
          ...
        }
      ]
    }
  }
}
```

---

### Apply Offer

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
      "items": [
        {
          "id": "I2",
          ...
          "quantity": {
            "count": 1
          }
        },
        {
          "id": "I2",
          ...
          "quantity": {
            "count": 1
          }
        }
      ],
      ...
    }
  }
}
```

Any offer that results in a change in the order value and/or quantity of items will be applied to the order quote and will be part of all order-level callbacks, i.e., `/on_init`, `/on_confirm`, `/on_status`, `/on_cancel`, `/on_update`:

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
          {
            ...
          },
          {
            "@ondc/org/item_id": "buy2get3",
            "@ondc/org/item_quantity": {
              "count": 1
            },
            "title": "buy 2 items, get 3rd for free or at offered price",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "0.00"
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
                      "value": "buyXgetY"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    },
                    {
                      "code": "additive",
                      "value": "no"
                    },
                    {
                      "code": "item_id",
                      "value": "I1"
                    },
                    {
                      "code": "item_count",
                      "value": "1"
                    },
                    {
                      "code": "item_value",
                      "value": "0.00"
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

---

### Reverse Offer
Since the "buyXgetY" offer resulted in additional quantity of the item, reversal of the offer will mean reducing this item quantity, the aggregate of which should not exceed the additional quantity of the item offered.

For example, the "buy2get3" offer above (Scenario 1) added 1 additional quantity of item I2; this means reversal of the offer can reduce, in aggregate, a maximum of 1 quantity of I2 from the order.

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
                  "value": "buy2get3"
                },
                {
                  "code": "currency",
                  "value": "INR"
                },
                {
                  "code": "value",
                  "value": "0.00"
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
            "@ondc/org/item_id": "buy2get3",
            "@ondc/org/item_quantity": {
              "count": 0
            },
            "title": "reversing the offer by changing quantity to 0",
            "@ondc/org/title_type": "offer",
            "price": {
              "currency": "INR",
              "value": "0.00"
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
                      "value": "buyXgetY"
                    },
                    {
                      "code": "auto",
                      "value": "yes"
                    },
                    {
                      "code": "additive",
                      "value": "no"
                    },
                    {
                      "code": "item_id",
                      "value": "I2"
                    },
                    {
                      "code": "item_count",
                      "value": "1"
                    },
                    {
                      "code": "item_value",
                      "value": "0.00"
                    }
                  ]
                }
              ]
            }
          }
        ],
        ...
      },
      ...
    }
  }
}
```
