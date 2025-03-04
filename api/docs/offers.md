# Offers

## Offer Construct - Includes:

### Applicability Criteria
Defines where/when/on what offer(s) may be applicable:
- **Location(s)**
- **Categories**
- **Item(s)**
- **Time Period Validity**

Applicability criteria for an offer will be:
- Intersection of the defined attributes, i.e., location(s), categories, and time period validity, if these are the attributes defined for the offer.
- Within a multi-valued attribute, applicability criteria will apply to the union of all values, e.g., for location L1 & L2, the offer applies to L1 or L2.

### Qualifier
Qualifying/eligibility criteria, based on cart value, i.e., aggregate value of all items in the cart, not including taxes, delivery, and packaging charges.

### Benefit
Benefit to the buyer from the offer(s).

### Meta Info
Other info, i.e., whether the offer requires explicit buyer opt-in, whether an offer can be applied in addition to other offer(s).

### Other Info
Includes offer ID/type, images, etc.

---

## Offer Flows

### Publish Offer
SNP publishes offers, per provider, in the full and/or incremental catalog refresh:
- BNP may display one or more of these offers on their app to enable buyer discovery.
- Offer definition is templatized below, and the BNP can create an appropriate description for an offer from the template variables (qualifier, benefit) and using the applicability criteria as defined above.

### Apply Offer
SNP applies offer(s) to the order quote during the buyer checkout flow:
- SNP may apply offer(s) without publishing in the full or incremental catalog refresh.
- Offer is applied as part of processing request (`/select`, `/init`):
  - SNP evaluates offers for which the buyer is eligible, regardless of the applicability of such offers, and sends details of these offers in `/on_select`.
  - SNP validates offer ID(s) for opt-in offers and applies the offer(s) to the cart selection.
  - Offers based on buyer identification can be applied while processing `/init`.
  - Auto-applied offers will be shown in the offer quote. If the buyer doesn’t want any auto-applied offer, they can remove the offer from the cart, and BNP will communicate this to SNP in the subsequent request (`/select`, `/init`).
- Offers resulting in monetary benefit to the buyer will be applied to the order quote:
  - Cart-level offers will be applied at the "order" level in the quote, and the attributes in "benefit" below will be part of the "offer" quote line item.
  - Allocation of order-level offers to individual line items is at the discretion of the BNPs, subject to:
    - The absolute aggregate of the offer reversal values cannot exceed the offer values applied at the time of order creation.
    - Any reversal of offer value due to cancellation/return, etc., will have to be explicitly defined in `quote_trail`.

### Reverse Offer
Whenever an offer qualifier is no longer valid in the post-order flow, SNP may decide to reverse the offer, in part or full, up to the benefit provided to the buyer in aggregate.

---

## Exception Conditions for Offers
- If the offer is no longer valid in pre-order APIs such as `/on_init` or `/on_confirm`, SNP can send error code `30006`.
- If the offer is valid but cannot be fulfilled, e.g., offer item not available for offers such as freebie, etc., SNP can send error code `30007`.
- If the offer is no longer valid or cannot be fulfilled in `/on_confirm`, SNP should cancel the order.

---

## Offer Template
In the template below (M - mandatory, O - optional, N/A - not applicable):

| # | Offer Type | Min Value | Item Count | Item Count Upper | Item ID | Value | Value Type | Value Cap | Item Count | Item ID | Item Value | Additive | Auto |
|---|------------|-----------|------------|------------------|---------|-------|------------|-----------|------------|---------|------------|----------|------|
| 1 | Discount   | O         | N/A        | N/A              | N/A     | M     | M          | O         | N/A        | N/A     | N/A        | M        | M    |
| 2 | BuyXGetY   | O         | M          | N/A              | N/A     | N/A   | N/A        | N/A       | M          | O       | M          | M        | M    |
| 3 | Freebie    | O         | N/A        | N/A              | N/A     | N/A   | N/A        | N/A       | M          | M       | N/A        | M        | M    |
| 4 | Slab       | O         | M          | M                | N/A     | M     | M          | M         | N/A        | N/A     | N/A        | M        | M    |
| 5 | Combo      | O         | N/A        | N/A              | M       | M     | M          | M         | N/A        | N/A     | N/A        | M        | M    |
| 6 | Delivery   | O         | N/A        | N/A              | N/A     | M     | M          | O         | N/A        | N/A     | N/A        | M        | M    |
| 7 | Exchange   | O         | N/A        | N/A              | N/A     | N/A   | N/A        | N/A       | N/A        | N/A     | N/A        | M        | M    |
| 8 | Financing  | O         | N/A        | N/A              | N/A     | N/A   | N/A        | N/A       | N/A        | N/A     | N/A        | M        | M    |

---

### Offer Type:
- **Discount**: Discount percent or amount applied to the cart value, with or without a cap, and may be based on minimum cart value.
- **BuyXGetY**: For every "X" item count in the cart, a total of "Y" items will be offered (i.e., "Y" - "X" additional items) at the offered value.
- **Freebie**: For a minimum order value, 1 or more free item(s) will be offered at 0 value.
- **Slab**: Distinct slabs for applicable discount for a range of item quantity.
- **Combo**: Discount for (primary) item and 1 or more (secondary) items.
- **Delivery**: Discount on delivery charges, with or without a cap.
- **Exchange**: 1 or more catalog items can be purchased in lieu of an existing item owned by the buyer.
- **Financing**: SNP financing available for 1 or more catalog items.

---

### Qualifier:
- **Min Value**: Minimum cart value for the offer to be applicable.
- **Item Count**: Minimum count of items in the cart for the offer to be applicable.
- **Item Count Upper**: Maximum count of items in the cart for the offer to be applicable.
- **Item ID**: (Secondary) item(s) for the offer.

---

### Benefit:
- **Value**: Discount on the cart value (-ve) offered.
- **Value Type**: "percent", "amount".
- **Value Cap**: Cap on the offer value.
- **Item Count**: Additional count of items offered.
- **Item ID**: Additional item offered, should be part of the catalog.
- **Item Value**: Offered value of the item.

---

### Meta:
- **Additive**: Whether the offer can be applied with other offers.
- **Auto**: Whether the offer is auto-applied or requires explicit opt-in by the buyer.