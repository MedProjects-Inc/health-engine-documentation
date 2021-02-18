# Patient ID, Visit Number and Item Code

Display formats and generation algorithm for patient MRN and Visit Number.

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

## Item Code

- Codes for Orderable Items
- Used primarily by the Ordering Module

### Format and Generation

- 8-digit **ALPHA-numeric** Code
- Department Code `DEP` + `5 digits`
  - `LAB` + `12345` = `LAB12345`
  - `RAD` + `34567` = `RAD34567`
  - `PHA` + `67890` = `PHA67890`
- No need to do special formatting for display.
  - Can be displayed as is.
- The last 5 digits are NOT auto-incremented in the system because these are externally maintained code sets.
  - In the future, we can have a UI facility to manage this code set where an auto-increment can be done.
  - But, FOR NOW, this code set will be managed externally (via Excel or CSV) and uploaded to the system.
