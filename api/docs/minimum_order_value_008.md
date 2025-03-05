### Minimum Order Value - 008

- SNP may specify a minimum order value per store.
- If the cart value is less than the minimum order value during checkout (`/select`, `/init`), SNP can return error code `30023` in the callback response. 
- BNP may use this to prompt the buyer to increase the order value to be above the minimum order value.

```json
{
  "context": {
    "action": "on_search",
    "core_version": "1.2.5",
    ...
  },
  "message": {
    "catalog": {
      "bpp/providers": [
        {
          "id": "P1",
          ...
          "tags": [
            {
              "code": "order_value",
              "list": [
                {
                  "code": "min_value",
                  "value": "300.00"
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