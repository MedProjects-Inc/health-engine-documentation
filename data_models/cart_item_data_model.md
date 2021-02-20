# Cart Item Data Model

This data model is used for the Cart Items in the Ordering Module.

- Latest version: [cart_item_data_model.json](cart_item_data_model.json)

## Descriptive Data Elements

***Data Model Information***

> No need to use them in transport if not needed. Added here as added labels, description and information.

- `data_model`: `Cart Item`. Hard coded.
- `data_model_code`: `cart_item`. Hard coded.
- `version`: `0.90`. Hard coded during update.
- `last_updated`: `FEB-18-2021`. Hard coded during update.

## Main Data Elements

***Patient IDs***

- `patient_id`: Integer. **Primary JSON Element** to be used for patient identifier.
- `sys_id`: Alternative to `patient_id`
- `patient_mrn`: Alternative to `patient_id`
- `patient_code`: Alternative to `patient_id`

***Visit IDs***

- `visit_number`: Integer. Primary JSON element to be used for patient identifier.
- `visit_id`: Alternative to `visit_number`
- `att_md_code`
- `att_md_name`

***(Orderable) Items***

> These are the items that are searched, added to cart and later on submitted as finalized **Orders**. They come from the Item Master, Drug Master and other Orderable Item Masters.

- `item_code`: e.g. `LAB67890`
- `item_description`: e.g. `Complete Blood Count`
- `item_category`: An internal categorization in the Master table
- `order_type_code`: e.g. `LAB`,
- `order_type_text`: e.g. `Laboratory Test`
- `service_section_code`:
- `service_section_text`:
- `service_department_code`:
- `service_department_text`:
- ~~`performing_unit`~~: DO NOT USE.
- ~~`service_unit`~~: DO NOT USE.

***Order Details (submitted to Performing Units once finalized)***

- `order_quantity`: (Optional). To be used only by supplies, medication and other applicable orders.
  - Default is `1`.
- `order_priority_code`: e.g. `R` for Routine
  - Default is `R` for `Routine`
- `order_priority_text`: e.g. `Routine`,
- `order_datetime`: Current datetime
- `order_effective_datetime`: When order is effective. Useful for advanced booking and scheduling.
- `order_request_datetime`: Same as `order_effective_datetime`
- `clinical_indication`: Can be a required field in some orders.
- `order_notes`: Remarks, additional details, special instructions and other notes

***Cart Details***

- `cart_add_datetime`: Datetime when added to cart
- `cart_status_code`: Status. Default is `N`.
  - No immediate use but might have future uses.
- `cart_status_text`: `New`
- `encoder_user_id`: 1,
- `encoder_username`: `N`,

***Ordering Doctor***

- `ord_md_id`: System identifier of MD
- `ord_md_code`: Same as `ord_md_id`
- `ord_md_name`
- `ord_md` object: This is a standard `doctor` data type object.

```json
"ord_md" {
    "role_code": "",
    "role_text": "",
    "doctor_id": "",
    "doctor_name": {
        "prefix": "Dr.",
        "first": "Mario",
        "middle": "Delgado",
        "last": "Villa",
        "suffix": "",
        "full": "Dr. Mario D. Villa",
        "display": "Dr. Mario D. Villa"
    },
    "specialty_code": "",
    "specialty_text": "",
    "department_code": "",
    "department_text": "",
    "is_adm_md": false,
    "is_att_md": false
}
```

---

## "Attached" Optional Data Elements

> These are special elements that are attached to the `cart_item` data model depending on specific conditions.
> 
> Some elements are optional at this point. For finalization when relevant modules are clearer.
> 
> These items will be fill out during the Ordering Process. Some items will be coming from dropdown list of values.

***Pricing***

> Optional at this point. For finalization when relevant modules are clearer.

- `pricing` object: For attaching price of item during ordering.

```json
"pricing": {
    "scheme_code": "",
    "scheme_text": "",
    "unit_price": ""
}
```

***Laboratory***

- `laboratory` object: If order is a **laboratory**.

```json
"laboratory": {
    "specimen_type_code": "",
    "specimen_type_text": "",
    "collection_site_code": "",
    "collection_site_text": ""
}
```

***Radiology***

- `radiology` object: If order is a **radiology**.

```json
"radiology": {
    "last_menstrual_period": "Date",
    "medical_implants": "Notes"
}
```

***Medications***

- `medication` object: If order is a **medication**.

```json
"medication": {
    "generic_name": "Amoxicillin",
    "s2_license_number": "",
    "frequency": "",
    "dosage": "",
    "route": "",
    "duration": "",
    "sig_instructions": ""
}
```

> For finalization of `medication` object. Will need to decide on additional data elements.

***Dietary, Supply, Procedure***

- `dietary` object: If order is a **dietary**.
- `supply` object: If order is a **supply**.
- `procedure` object: If order is a **procedure**.

```json
"dietary": {},
"supply": {},
"procedure": {}
```

Last updated: Feb-19-2021