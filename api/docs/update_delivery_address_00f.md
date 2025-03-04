## Update Delivery Address - 00F

Delivery address, contact person, and phone number can be updated. Updated delivery address details will become a part of the order & will be available in all post-order API callbacks, i.e., `/on_status`, `/on_update`, `/on_cancel`.

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
          "end": {
            "location": {
              "gps": "12.4535,77.9283",
              "address": {
                "name": "my house or door or floor #",
                "building": "my building name or house #",
                "locality": "my street name",
                "city": "Bengaluru",
                "state": "Karnataka",
                "country": "IND",
                "area_code": "560037"
              }
            },
            "person": {
              "name": "Buyer 1"
            },
            "contact": {
              "phone": "9886098860"
            }
          }
        }
      ]
    }
  }
}
```

---
