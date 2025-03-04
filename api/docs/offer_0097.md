## Offer (Exchange) - 0097

## SNP Publishes Offer
SNP publishes an offer, either as a full or incremental catalog refresh. The exchange flow is defined below:

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
              "id": "exchange1",
              "descriptor": {
                "code": "exchange",
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
          ..
        }
      ]
    }
  }
}
```

## Offer (Financing) - 0098

SNP publishes offer, in full or incremental catalog refresh; financing by SNP/seller will be in the SNP payment collection flow, defined here:

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
              "id": "financing1",
              "descriptor": {
                "code": "financing",
                "images": [
                  "https://snp.com/images/offer2-banner.webp"
                ]
              },
              "location_ids": [
                "L1"
              ],
              "category_ids": [
                ..
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
          ..
        }
      ]
    }
  }
}
```
