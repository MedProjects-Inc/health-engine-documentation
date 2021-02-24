# Data Models Documentation and Reference

Data models in Health Engine JSON files are REFERENCES for the different data object that need to be transmitted. They document the standards for what data elements are available, along with how each data is named.

Some guidelines:

- The data models are NOT SENT '***as is***'. They are to be used as a REFERENCE for data build-up of JSON API Responses and Event Messages.
- Some JSON elements are there for descriptive purposes only. Using or sending them in JSON APIs or Event Messages are **OPTIONAL**.
- Some JSON elements are there as ALTERNATIVES for main data element, e.g. `patient_code` as alternative for primary element `patient_id`.

## These are the Data Models with their description files (.md)

- [patient_data_model.json](patient_data_model.json) / [patient_data_model.md](patient_data_model.md)
- [visit_data_model.json](visit_data_model.json) / [visit_data_model.md](visit_data_model.md)
- [cart_item_data_model.json](/data_models/cart_item_data_model.json) / [cart_item_data_model.md](cart_item_data_model.md)
- [order_item_data_model.json](order_item_data_model.json) / [order_item_data_model.md](order_item_data_model.md)
