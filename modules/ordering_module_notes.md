# Notes on Ordering Module

## General Ordering Module Functions

- `search_orderable_items`
- `add_to_cart`
- `edit_cart_item`
- `remove_from_cart`
- `show_cart`
- `submit_cart`
- `show_orders`
- `cancel_order`
- `request_cancel_order`
- `approve_request_cancel_order`

---

## Submit Cart to Order Transactions

- `submit_cart`: When the cart is finalized and submitted, the following ids are generated in the orders table:
  - Order Group ID (`order_group_id`): This identifies the batch from the cart.
  - Order Item ID (`order_item_id`): The primary key for each item in the transaction.
- These `ids` are important identifiers that will be passed to billing, inventory and HL7 messaging.

---

## Order Types

Each `order_item` is assigned an `order_type`. There are several **Order Types**. Depending on the `order_type`, there might be additional fields, forms validation and processing algorithms for each `order_item`.

The different Order Types for HIS Version 1 include:

- Pharmacy
- Supplies
- Laboratory
- Radiology
- Packages
- Diet

Additional Order Types can be considered for future HIS Versions:

- Pharmacy (Pharmacy may be further split into specific subtypes)
  - Drugs/Medications
  - IV Fluids
- Patient Care/Nursing Orders
- Administrative Order
- Use of Equipment
- Order Sets

> What is the difference between **Packages** and **Order Sets**?
> 
> - Packages are "financial" or "billing" grouping. Their prices may be modified/discounted based on their inclusion into the package, e.g. Wellness Package.
>	- You cannot take out an item within the packages.
> - Order Sets are commonly-ordered or related orderable items that are grouped together for convenience or for clinical relevance, e.g. clinical pathways.
>	- When they are submitted to the Order Cart, they are 'disasembled' from the set and are placed individually in the cart. This allows for deletion/removal of items from within the cart.

---

## Payment Policy Considerations for Ordering

> Everything here is for review. Not to be developed yet.

### `FOR REVIEW` Payment First Policy

- This is a policy where a procedure, test or order is not sent to Performing Units unless payments have been made.
- Usually done in outpatient or cash-basis transactions.
- Possible behavior:
  - Items added to cart, then submitted as orders.
  - Orders are sent to charging modules for billing and payment.
  - Once payment has been made, message is sent back to Orders.
  - An event message is triggered to send to Performing Units.

### `FOR REVIEW` Performance First Policy

- This is another policy where items are NOT charged or posted in billing unless they have been performed, e.g. X-ray was taken or lab test was done.
- Usually done for Inpatients or ER patients.
  - Some orders are submitted but not performed or carried out. Patient shouldn't pay for those.
- Possible behavior:
  - Users order procedures/supplies for patient.
  - Performing units receive orders.
  - If order is carried out, then ordering/charging module is triggerd to post charges.
  - If order is not carried out, then performing unit can cancel order prior to discharge.

---

## Event Messages

These service units, modules and other systems need to be informed once an `ORDER` transaction has occured.

- **Performing Unit**: The one that needs to do the task.
  - Via **WORKLIST module**
- **Charging/Billing Module**
- **Alpha Mirth**: for events tracking and sending to other systems
- Other systems via Alpha Mirth
  - LIS
  - RIS
  - Other Ancillary IT Systems

### Event Triggers

1. `order_submitted`: When an order is finalized and submitted
2. `order_cancelled`: When an order is cancelled, whether from Ordering Module or Charging Module.