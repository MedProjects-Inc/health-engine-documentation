# Notes on Patient Administration, Encounter Management and MPI

## Patient ID

- Named `patient_id` in JSON
- Also called the PIN (Patient Identification Number) or Medical Record Number (MRN)
- Should accommodate generation from **a multi-site clinic/hospital setup**
  - where several clinics can generate their own MRN
  - but patient demographics are sent to a centralized MPI that recognizes all MRNs from different sources in the network
- 10-digit number
- `site code` + `reserve number` + `auto-increment 7`
  - `11` + `1` + `1234567` = `1111234567`
  - `32` + `8` + `4567890` = `3284567890`
  - `10` + `1` + `2345678` = `101234567`
- `site code` - 2-digit code assigned to the site
  - Cannot start with zero, e.g. `01`
  - Default is `10`
- `reserve number` - can have special uses
  - Can be reserved for 3 digit site code
  - Can be used for testing purposes
  - Default is `1`
- **Display** should be spaced in 3-3-4 intervals
  - `111 123 4567`
  - `328 456 7890`
- Should be stored as full integers for easy storage, sorting and retrieval

---

## Visit Number

- Named `visit_number` in JSON.
- Also called the Visit ID `visit_id`, Visit Code, Encounter Number or Encounter Code.
- 10-digit number
- `current date YYMMDD` + `auto-increment 4`
  - `210201` + `1234` = `2102011234` (Feb-01-2021)
  - `210212` + `6789` = `2102126789` (Feb-12-2021)
- Should be stored as full integers for easy storage, sorting and retrieval
- No need to do special formatting for display.
  - Can be displayed as is.

---

## Some Generic and Advanced Functions

### Patient Temporary Patient (`register_temporary_patient`)

There are instances where it's needed to quickly generate a temporary identity in the system for a patient, e.g. emergency, unconscious, no identity.

Parameters needed for generation:

- Sex: Male (`M`) or Female (`F`)
- Estimated age (integer): Used to compute `date_of_birth`

Actions:

- Generate an ACTIVE `patient_id`. No need to create a temporary one.
- Generate a temporary name:
  - Type: `Temporary`
  - Last Name: `Changeme`
  - First Name (Male): `John` + ` ` + `000` (Auto-generated Number)
  - First Name (F): `Jane` + ` ` + `000` (Randomly-generated Number)
  - Middle Name: Randomly-generated 3 letters, e.g. `Mut`, `Xyz`, `Pth`
  - Sample Output:
    - `CHANGEME, John 298 Mut`
    - `CHANGEME, Jane 357 Xyz`
- Generate a temporary date of birth:
  - Compute only year from age.
  - Use January 1 as default month and day.

---
