# Force Cancellation - 005  

## Overview  
Considering that `/cancel` is a request by **BNP** for **SNP** to cancel an order/fulfillment, there may be a need for BNP to initiate **"force cancellation"** in specific cases, such as:  
- **Buyer requesting cancellation** due to fulfillment **TAT breach**.  

## Cancellation Terms  
SNP may define **cancellation terms**, which can include a **cancellation fee** as:  
- A **flat amount** or a **percentage (%)**:  
  - `%` of **order value** (excluding taxes) for **order cancellation**.  
  - `%` of **fulfillment value** (i.e., item + fulfillment costs, excluding taxes) for **fulfillment cancellation** (in case of multiple fulfillments).  
- For orders with **multiple fulfillments**, the **cancellation fee** may be defined as a percentage.  

### Where Cancellation Terms Can Be Defined  
Cancellation terms can be optionally **defined at:**  
- **Order level** (`/on_init`).  
- Applicable for **order/fulfillment cancellation**, only after the order state is **"Accepted"**.  

---

## Cancellation Terms Definition  
- **`/on_init`**:  
  - Cancellation fee is defined for **specific fulfillment states & reason codes**.  
  - Can include a **wildcard ("*")** as a **catch-all clause** for undefined fulfillment state & reason code combinations.  

## Cancellation Fee Calculation  
- **For `/on_cancel` response:**  
  - Cancellation fee = `(order value - updated order value in /on_cancel)`.  
- **For no `/on_cancel` response:**  
  - If **matching fulfillment state & reason code**, the cancellation fee is as per `/on_init`.  
  - If **no match found**, cancellation fee = `0`.  

---

## Cancellation Process  
1. **BNP initiates cancellation** using `/cancel` (with **reason code** / **fulfillment ID**, as applicable).  
2. **Includes TAT** (Turnaround Time) for receiving a valid cancellation response.  
3. **If SNP sends a valid `/on_cancel` response within TAT:**  
   - Response includes **updated quote** & **order/fulfillment state set to "Cancelled"**.  
   - BNP **processes response** and calculates the cancellation fee as per the defined terms.  
4. **If SNP does not send a valid response within TAT:**  
   - BNP sends a **force cancel request** (`/cancel` with `"force": "yes"`).  
   - If **SNP responds within TAT**, processing follows the steps in point **3**.  
   - If **SNP does not respond**, BNP will:  
     - **Create an IGM issue for resolution** after TAT expiry.  
     - Include details like **cancellation fee calculated** & **refund issued (if applicable)**.  
     - Resolution of the **IGM issue** will generate a valid `/on_cancel` response for the force cancellation.  
