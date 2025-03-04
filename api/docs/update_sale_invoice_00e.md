## Update Sale Invoice - 00E

Update sale invoice for cases where BNP generates an invoice (e.g., in case of F&B ISN).

### Payload Changes
```json
{  
  "context": {
    "action": "update",
    "core_version": "1.2.5",
    ..
  },
  "message": {
    "update_target": "fulfillment",
    "order": {
      "id": "O1",
      "fulfillments": [
        {
          "id": "F1",
          ..
          "tags": [
            {
              "code": "update_sale_invoice",
              "list": [
                {
                  "code": "url",
                  "value": "https://invoice_url"
                }
              ]
            }
          ]
        }
      ]
    }
  }
}
```

---
