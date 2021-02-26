# Notes on Patient Administration, Encounter Management and MPI


## Patient ID, Visit Number Generation

Please refer to the [Algorithms](../docs/algorithms.md) page.

## Some Generic and Advanced Functions

### Patient Temporary Patient (`register_temporary_patient`)

There are instances where it's needed to quickly generate a temporary identity in the system for a patient, e.g. emergency, unconscious, no identity.

Parameters needed for generation:

- Sex: Male (`M`) or Female (`F`)
- Estimated age (integer): Used to compute `date_of_birth`

Actions:

- Generate an ACTIVE PID (`patient_id`). No need to create a temporary one.
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

### Merge Patients

Parameters:

- Active PID: The one that is retained and kept active.
- Inactive PIDs (can be more than 1): The ones that need to be merged and deactivated.

Actions:

- Add active and inactive PIDs in an index/mapping table
- Deactivate patient accounts with inactive PIDs
- Add the active PID in the Add active MRN in `active MRN` column for the inactive patient accounts