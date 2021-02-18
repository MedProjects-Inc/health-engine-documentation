# JSON Documentation and Other Stuff

## JSON Style Guide

Some guidelines on JSON formatting, element naming and other styles.

### Snake Case for Variable Names

- Use **snake_case** for variables. It is easier to read.
  - `date_of_birth`, `is_default`

### Boolean Properties

- Use `is_` prefix for configuration, validation or description booleans.
  - `is_default`
  - `is_primary`
- `has_` prefix can be used **sparingly** if it makes more sense.
  - `has_allergies`

### Decimal Values

- Numerical values that are NOT integers should be written down as strings, e.g. decimals, to avoid loss in precision from possible conversion.
  - `"7.01" vs. 7.01`

### Command, Requests and Events

- Some of our JSON messages will contain very specific commands, especially for event messages and syncing.
- The preferred format is `command_noun`.
- Here are generic samples:
  - `add_record`
  - `update_record`
  - `delete_record`
  - `deactivate_record`
  - `reactivate_record`

### Reserved Suffixes

#### `_notes`

- If a field for remarks is needed (in a set of data elements), use `_notes`.
- Samples:
  - `allergy_notes`
  - `order_notes`

#### `_display`

- To be used for formatting changes in a specific data element, e.g. `date_of_birth` ("2020-10-19") is reformatted for HTML display as `date_of_birth_display` ("OCT-19-2020").
- Can be used for patient names if reformatting is needed.
- Can also be used for medication when TALLman lettering is needed.

#### `_base64`

#### `_url`

#### `_id`

#### `_code`, `_text` (coded text data type) with `_display`

---

## JSON Document Types

There are 3 Major JSON Document Types that we need to maintain:

1. API `GET` Response / `POST` Payload
2. Event Messages
3. Object/Resource Data Models

Here are more details:

### JSON API `GET` Response / `POST` Payload

- These are JSON responses from our APIs between the front-end and back-end, across different modules and across different applications, e.g. Mobile App.
- This is the one that can be accessed, documented or tested via Postman, Swagger, OpenApi or Tiiny.site.
- We need to standardize these JSON APIs.
- They may be unique per microservice/component.

### Event Messages

- Event Messages are similar to JSON APIs but they are the ones specifically sent out to other applications **via messaging**, e.g. HL7 or JSON.
- These are commonly sent to Alpha Mirth.
- If you need **to sync databases across different applications**, e.g. Patients, Users, Results, you need to think of **Event Messages**.

### Object/Resource Data Models

- These are JSON Representations of different objects and resources across the architecture and system.
  - Sample objects: patients, visits, orders, doctors
- These data models create **common standards**, **structures** and **naming conventions** for use by APIs and Event Messages.
- These JSON files ARE NOT sent as is across the different modules/applications.
  - They are embedded, as needed, in the different JSON files.
  - Think of them like HL7 Segments, e.g. PID, PV1
- You don't need to send the full data model. The application can just send the minimum required/requested properties and fields.
  - If only name and date of birth are needed, only those 2 fields can be included in the JSON API response or Event Message.

## Generic JSON Payload

The generic JSON payload is inspired by [JSON:API](https://jsonapi.org/format/) concepts. This will be the generic transport format for `GET` responses, (some) `POST` messages and ***Event*** messages.

### The 3 main components are

- `meta`: This contains information for the JSON request/response itself. This corresponds (or is similar) to the `MSH` segment of HL7 v2 messages. The minimum components are:
  - `id`: Unique identifier for the message or response.
  - `type`: A unique name for tracking purposes, e.g. `patient_merged`
- `data`: This is where the whole data in JSON format is attached.
- `errors`: (Optional) This **only required** when there is an error from the server or application. It is an array of errors. If this is sent, `data` should not be sent.

### Here are samples of the Generic JSON Payload

For **DATA** payloads:

```json
{
  "meta" : {
    "id" : "",
    "type" : ""
  },
  "data" : {}
}
```

For **ERROR** responses:

```json
{
  "meta" : {
    "id" : "",
    "type" : ""
  },
  "errors" : [
    {
      "id" : "UUID",
      "status" : "404",
      "code" : "Not Found",
      "title" : "Not Found",
      "detail" : "A server MUST return 404 Not Found when processing a request to modify a resource that does not exist."
    }
  ]
}
```

Sample Patient with Visit:

```json
{
  "meta": {
    "id": "",
    "type": ""
  },
  "data": {
    "patient": {
      "patient_mrn": 1011234567,
      "patient_code": "1011234567",
      "patient_id": "1011234567",
      "date_of_birth": "YYYY-MM-DD",
      "date_of_birth_display": "MMM-DD-YYYY",
      "is_active": true,
      "is_merged": false,
      "sex_code": "M",
      "sex_text": "Male",
      "sex_display": "Man"
    },
    "visit": {
      "patient_mrn": 1011234567,
      "visit_number": 2102126789,
      "admit_datetime": "",
      "discharge_datetime": "",
      "patient_class_code": "I",
      "patient_class_text": "Inpatient",
      "location": {
        "location_code": "",
        "location_text": "",
        "type_code": "",
        "type_text": "",
        "bed": "",
        "room": ""
      }
    }
  }
}
```
