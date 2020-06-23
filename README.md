PSA Database Handbook
================
Last Updated: 06/05/2020 by Aurelie

<br>

1. Nomenclature and Definition
==============================

<br> **Tables:**

<img src="Additional/nomenclature_and_definition_tables.png" id="id" class="class" style="width:30.0%" style="height:30.0%" />

<br> **Constraints:**

<img src="Additional/nomenclature_and_definition_constraints.png" id="id" class="class" style="width:45.0%" style="height:45.0%" />

<br> **Missing Data**:

-   **NA** if data is missing because data collection is ongoing.
-   **-999** (for numbers and text) and **1900-01-01** (for dates) if data collection is over and data is missing.

<br> **Naming Convention**:

-   All table names and columns are in lowercase.
-   Words in table and column names were delimited using underscores.
-   Date-time format is alway *timestamp without time zone* and time zone is always GMT.
-   All coordinates are expressed in the WGS84 latitude, longitude format, with latitude and longitude data being stored in two different columns.

2. Site Information and Producer IDs
====================================

<br> **Description:**

-   Data in the `producer_ids` and `site_information` tables are collected using the tech dashboard. New rows will be added to these tables during onboarding. Existing rows will be updated by the data creator directly from the tech dashboard without requiring approval of a data shepherd. There are no validation columns associated with these two tables.

-   When onboarding a new site, the user will have the choice between creating a new producer (or partner) or updating the contact information of an existing producer (or partner). New codes will be automatically associated to a site during onboarding.

-   Unique producer IDs are attributed using the following format: `yyyysssssxxx` with `yyyy`: year of entry in network, specified using 4 digits; `sssss`: affiliated abbreviation (up to 5 characters); and `xxx`: up to 3-digit numbers.

-   *For PSA on-farm sites:* Site codes are defined using 3 letters.

-   *For partner sites:* Site codes are defined using 2 letters plus 1 digit ranging from 1 to 9 (zeros were excluded to avoid confusions with the letter O).

-   Data strings stored in the *code*, *affiliation*, and *county* columns are all uppercase.

-   The `collaboration_status` table and *collaboration\_status* column in the `affiliations` table states if the producer is affiliated with a university or an industry partner.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_site_info_and_producer_ids.png" id="id" class="class" style="width:70.0%" style="height:70.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`collaboration_status`:

| Columns               |  Data Type  | Unit | Description                     |
|-----------------------|:-----------:|:----:|---------------------------------|
| collaboration\_status | charvar(15) | *NA* | University or industry partner. |

`affiliations`:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>affiliation</td>
<td align="center">charvar(25)</td>
<td align="center"><em>NA</em></td>
<td>Lists all states and partners abbreviations.</td>
</tr>
<tr class="even">
<td>label</td>
<td align="center">charvar(50)</td>
<td align="center"><em>NA</em></td>
<td>Labels corresponding to states and partners abbreviations.</td>
</tr>
<tr class="odd">
<td>collaboration_status</td>
<td align="center">charvar(15)</td>
<td align="center"><em>NA</em></td>
<td>University or industry partner.</td>
</tr>
</tbody>
</table>

`producer_ids`:

| Columns      |  Data Type  | Unit | Description                       |
|--------------|:-----------:|:----:|-----------------------------------|
| producer\_id |   char(9)   | *NA* | Unique producer ID.               |
| last\_name   | charvar(20) | *NA* | Grower's last name.               |
| email        | charvar(50) | *NA* | Grower's up-to-date email.        |
| phone        |   char(10)  | *NA* | Grower's up-to-date phone number. |

`site_information`:

| Columns             |  Data Type  |      Unit      | Description                               |
|---------------------|:-----------:|:--------------:|-------------------------------------------|
| cid                 |    serial   |      *NA*      | Unique row ID (ignore).                   |
| code                |   char(3)   |      *NA*      | Site code.                                |
| producer\_id        |   char(9)   |      *NA*      | Producer ID associated to site code.      |
| affilitation        |   char(25)  |      *NA*      | Affiliated state or partner.              |
| year                |   char(4)   |      *NA*      | Year of the experiment.                   |
| state               |   char(2)   |      *NA*      | State.                                    |
| county              | charvar(30) |      *NA*      | County.                                   |
| address             |     text    |      *NA*      | Address of experimental site.             |
| latitude            |     real    | decimal degree | Latitude of experimental site \[WGS84\].  |
| longitude           |     real    | decimal degree | Longitude of experimental site \[WGS84\]. |
| notes               |     text    |      *NA*      | Optional notes.                           |
| additional\_contact |     text    |      *NA*      | Optional additional contact.              |

3. Protocol Enrollment
======================

<br> **Description:**

-   The `protocol_status` table lists all the existing PSA on-farm protocols and defines which protocols are currently active within the on-farm network. This table will need to be updated when a new protocol is being created and launched within the network, and when an existing protocol is being retired.

-   The `protocol_enrollment` table lists which protocols were/will be conducted at every site. New rows will be created when a new site is onboarded and data in this table will be completed when the field team selects protocols (by site) in the tech dashboard. The field team should only be able to see and select active protocols in the tech dashboard. New columns will need to be added when new protocols are launched.

-   There are no validation columns associated with these two tables and the field team will be able to update information provided in `protocol_enrollment` directly from the tech dashboard without requiring approval of a data shepherd. The `protocol_status` table will not be accessible to the field team via the tech dashboard.

-   The data flow team needs to think about how new protocols will be added to these two tables (new rows in `protocol_status` and new columns in `protocol_enrollment`).

-   In the `protocol_enrollment` table, default values in the following columns: *soil\_nitrogen* and *bulk density* are set to zero because these protocols have been retired. The data flow team will need to remember setting default values to zeros when retiring a protocol. Default values in the columns: *farm\_history* were set to one because that protocol is mandatory and information must be collected for all PSA on-farm sites.

-   The data flow team will have to think about the steps that need to be taken when a field team member select a protocol that was not initially selected during onboarding. Indeed if a protocol is not selected at onboarding, tables will still be pre-filled, but they will be pre-filled with missing values and these will need to be updated to null values (and zeros in the validation columns).

<br> **Relational Diagram:**

<br> <img src="Additional/RD_protocol_enrollment.png" id="id" class="class" style="width:40.0%" style="height:40.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`protocol_status`:

| Columns    |  Data Type  | Unit | Description                                      |
|------------|:-----------:|:----:|--------------------------------------------------|
| protocol   | charvar(30) | *NA* | Lists all existing PSA on-farm protocols.        |
| is\_active |   integer   | *NA* | Equals 1 if protocol is active, and 0 otherwise. |

`protocol_enrollment`:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cid</td>
<td align="center">serial</td>
<td align="center"><em>NA</em></td>
<td>Unique row id (ignore).</td>
</tr>
<tr class="even">
<td>code</td>
<td align="center">char(3)</td>
<td align="center"><em>NA</em></td>
<td>Site code.</td>
</tr>
<tr class="odd">
<td>farm_history</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if farm history data are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="even">
<td>in_field_biomass</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if in field biomass data are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="odd">
<td>decomp_biomass</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if decomp biomass data are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="even">
<td>soil_texture</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if soil texture data are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="odd">
<td>cash_crop_yield</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if cash crop yield data are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="even">
<td>gps_locations</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if plot corners and subplot GPS locations are recorded at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="odd">
<td>sensor_data</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if TDR sensor data are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="even">
<td>weed_research</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if weed pictures are collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="odd">
<td>bulk_density</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if surface bulk density measurements were collected at corresponding site, and 0 otherwise.</td>
</tr>
<tr class="even">
<td>soil_nitrogen</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Equals 1 if soil nitrogen core samples were collected at corresponding site, and 0 otherwise.</td>
</tr>
</tbody>
</table>

4. Farm History
===============

<br> **Description:**

-   Data in the `farm_history` table will be collected using Kobo. There are validation columns in this table. The field team will be able to suggest edits in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   Data validation will be performed for each site code. By default, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 0. After validation by the data creator and data shepherd, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 1. If `farm_history` data for a given code are validated by both the data creator and the data shepherd while some values are still missing, then all missing values will be converted to -999 or 1900-01-01. If all `farm_history` data is missing for a given site and data is validated by the data creator and shepherd, then all values are being converted to -999 or 1900-01-01 and values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that code are set to -999. The *farm\_history* protocol is mandatory, and if values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns for `farm_history` are set to -999, we consider that this particular site is not part of the study anymore.

-   Rows in the `farm_history` table will be pre-filled with empty values (and zeros in the validation columns) at onboarding. One row should be created for each farm code.

-   *kill\_time:* equals -1 if cover crop was killed before cash crop planting. Equals 0 if cover crop was killed at cash crop planting. Equals 1 if cover crop was killed after cash crop planting.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_farm_history.png" id="id" class="class" style="width:55.0%" style="height:55.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`cash_crops`:

| Columns    |  Data Type  | Unit | Description           |
|------------|:-----------:|:----:|-----------------------|
| cash\_crop | charvar(20) | *NA* | Lists all cash crops. |

`cc_planting_methods`:

| Columns              |  Data Type  | Unit | Description                            |
|----------------------|:-----------:|:----:|----------------------------------------|
| cc\_planting\_method | charvar(20) | *NA* | Lists all cover crop planting methods. |

`cc_termination_methods`:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cc_termination_methods</td>
<td align="center">charvar(35)</td>
<td align="center"><em>NA</em></td>
<td>Lists all cover crop termination methods.</td>
</tr>
</tbody>
</table>

`farm_history`:

| Columns                    |  Data Type  |  Unit  | Description                                            |
|----------------------------|:-----------:|:------:|--------------------------------------------------------|
| cid                        |    serial   |  *NA*  | Unique row id (ignore).                                |
| code                       |   char(3)   |  *NA*  | Site code.                                             |
| previous\_cash\_crop       | charvar(20) |  *NA*  | Last cash crop grown prior to cover crop.              |
| next\_cash\_crop           | charvar(20) |  *NA*  | Cash crop grown after cover crop.                      |
| cc\_planting\_date         |     date    |  *NA*  | Cover crop planting date.                              |
| cc\_termination\_date      |     date    |  *NA*  | Cover crop termination date.                           |
| cash\_crop\_planting\_date |     date    |  *NA*  | Cash crop planting date.                               |
| kill\_time                 |   integer   |  *NA*  | Cover crop kill time relatively to cash crop planting. |
| row\_spacing               |     real    |   in   | Cash crop row spacing.                                 |
| subsoiling                 |   integer   |  *NA*  | Equals 1 if site was subsoiled, 0 otherwise.           |
| strip\_till                |   integer   |  *NA*  | Equals 1 if site was strip-tilled, 0 otherwise.        |
| cc\_planting\_method       | charvar(20) |  *NA*  | Cover crop planting method.                            |
| cc\_termination\_method    | charvar(35) |  *NA*  | Cover crop termination method.                         |
| cc\_total\_rate            |   integer   | lbs/ac | Cumulative cover crop seeding rate.                    |
| total\_n\_previous\_crop   |     text    |  *NA*  | Total nitrogen applied to previous cash crop.          |
| total\_applied\_n\_rate    |     text    |  *NA*  | Total nitrogen applied to following cash crop.         |
| is\_irrigated              |   integer   |  *NA*  | Equals 1 if site was irrigated, 0 otherwise.           |
| validated\_by\_creator     |   integer   |  *NA*  | Defines validation by data creator.                    |
| validated\_by\_shepherd    |   integer   |  *NA*  | Defines validation by data shepherd.                   |

<br>

5. Cover Crop Mixture and Applied Chemicals
===========================================

<br> **Description:**

-   The `cc_mixture`, `cc_species`, `cc_families`, and `cc_mixture_validation` tables characterize the cover crop planted at each site. The `applied_chemicals`, `chemical_names`, `chemical_families`, and `chemical_validation` tables list the chemicals applied to each site (when applicable). Data in these tables will be collected using Kobo. After data collection, the field team will be able to suggest edits in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   Validation columns are associated with the `cc_mixture` and `applied_chemicals` tables. Because growers are free to plant any number of cover crops in their field and apply any combination of chemicals, validation of the `cc_mixture` and `applied_chemicals` data is completed in separate tables: `cc_mixture_validation` and `chemical_validation`, respectively. Validation will be performed by site code. By default, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 0. If at least one row/observation is recorded in the `cc_mixture` and `applied_chemicals` tables for a given site and that information was validated by the data creator and shepherd, then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that particular site are set to 1. If the data collection is ongoing or the data was not validated by the data creator and shepherd, then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that particular site remain equal to 0. If data is just missing for that particular site, and the missing data is being validated by the data creator and shepherd, then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that site are set to -999.

-   Rows in the `cc_mixture_validation` and `chemical_validation` tables will be pre-filled with empty values (and zeros in the validation columns) at onboarding for every site. One row should be created for each code. The `cc_mixture` and `applied_chemicals` table will not be prefilled. Rows will be added at parsing. The data flow team needs to be diligent to delete obsolete rows when a cover crop specie or chemical was mentioned by the field team by error, and then removed.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_cc_mixture_and_chemical.png" id="id" class="class" style="width:85.0%" style="height:85.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`cc_families`:

| Columns    |  Data Type  | Unit | Description                    |
|------------|:-----------:|:----:|--------------------------------|
| cc\_family | charvar(20) | *NA* | Lists all cover crop families. |

`cc_species`:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cc_specie</td>
<td align="center">charvar(20)</td>
<td align="center"><em>NA</em></td>
<td>Lists all cover crop species.</td>
</tr>
<tr class="even">
<td>cc_family</td>
<td align="center">charvar(20)</td>
<td align="center"><em>NA</em></td>
<td>Associates cover crop species to cover crop family.</td>
</tr>
</tbody>
</table>

`cc_mixture`:

| Columns    |  Data Type  |  Unit | Description                                      |
|------------|:-----------:|:-----:|--------------------------------------------------|
| cid        |    serial   |  *NA* | Unique row id (ignore).                          |
| code       |   char(3)   |  *NA* | Site code.                                       |
| cc\_specie | charvar(20) |  *NA* | Cover crop species planted in site.              |
| rate       |   integer   | lb/ac | Seeding rate of corresponding cover crop specie. |

`cc_mixture_validation`:

| Columns                 | Data Type | Unit | Description                  |
|-------------------------|:---------:|:----:|------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).      |
| code                    |  char(3)  | *NA* | Site code.                   |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd. |

`chemical_families`:

| Columns          |  Data Type  | Unit | Description                  |
|------------------|:-----------:|:----:|------------------------------|
| chemical\_family | charvar(30) | *NA* | Lists all chemical families. |

`chemical_names`:

| Columns          |  Data Type  | Unit | Description                              |
|------------------|:-----------:|:----:|------------------------------------------|
| chemical         | charvar(50) | *NA* | Lists all chemicals.                     |
| chemical\_family | charvar(30) | *NA* | Associates chemicals to chemical family. |

`applied_chemicals`:

| Columns  |  Data Type  | Unit | Description                             |
|----------|:-----------:|:----:|-----------------------------------------|
| cid      |    serial   | *NA* | Unique row id (ignore).                 |
| code     |   char(3)   | *NA* | Site code.                              |
| chemical | charvar(50) | *NA* | Chemical applied to cover crop in site. |

`chemical_validation`:

| Columns                 | Data Type | Unit | Description                  |
|-------------------------|:---------:|:----:|------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).      |
| code                    |  char(3)  | *NA* | Site code.                   |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd. |

6. In-Field Biomass
===================

<br> **Description:**

-   The `biomass_in_field` table records the fresh biomass data collected in the field and data in these tables will be collected using Kobo. There are validation columns in this table. The field team will be able to suggest edits in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   The `biomass_nir` table contains results of the NIR lab analysis conducted on the fresh biomass samples. Results from the NIR lab will be uploaded into the database via an upload functionality in the tech dashboard. There are validation columns in this table. Data should not need to be updated after import, provided that the uploaded file is in the proper format and there was no mistakes with sample ids.

-   Rows in the `biomass_in_field` and `biomass_nir` tables will be pre-filled with empty values (and zeros in the validation columns) at onboarding when the *in\_field\_biomass* protocol is selected for a given site. Rows will be pre-filled with missing values (including values in the validation columns) if the *in\_field\_biomass* protocol is not selected at onboarding. 2 rows should be created in each table for each site code, whether or not the *in\_field\_biomass* protocol is selected at onboarding.

-   Data validation will be performed for each observation/row in the data tables. After validation by the data creator and data shepherd, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 1. If the `biomass_in_field` data are validated by both the data creator and the data shepherd while some values are still missing, then all missing values will be converted to -999. If all `biomass_in_field` data is missing for a given site or if NIR analysis will not be conducted on a particular sample, then all values are converted to -999 and values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that code are set to -999.

-   *legumes\_40:* Visual appreciation of the percentage of legumes in the cover crop. Equals 1 if legumes represent more than 40% of the cover crop, 0 otherwise. This information is necessary for the cover crop analysis.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_in_field_biomass.png" id="id" class="class" style="width:50.0%" style="height:50.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`subplot`:

| Columns | Data Type | Unit | Description        |
|---------|:---------:|:----:|--------------------|
| subplot |  integer  | *NA* | List all subplots. |

`biomass_in_field`:

| Columns                 | Data Type | Unit | Description                                      |
|-------------------------|:---------:|:----:|--------------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                          |
| code                    |  char(3)  | *NA* | Site code.                                       |
| subplot                 |  integer  | *NA* | Subplot in site.                                 |
| fresh\_wt\_a            |    real   |   g  | Fresh weight of sample A.                        |
| fresh\_wt\_b            |    real   |   g  | Fresh weight of sample B.                        |
| bag\_wt                 |    real   |   g  | Empty bag weight.                                |
| legumes\_40             |  integer  | *NA* | Is there more than 40% of legumes in cover crop? |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.                      |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd.                     |

`biomass_nir`:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cid</td>
<td align="center">serial</td>
<td align="center"><em>NA</em></td>
<td>Unique row id (ignore).</td>
</tr>
<tr class="even">
<td>code</td>
<td align="center">char(3)</td>
<td align="center"><em>NA</em></td>
<td>Site code.</td>
</tr>
<tr class="odd">
<td>subplot</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Subplot in site.</td>
</tr>
<tr class="even">
<td>percent_fat, percent_cp, percent_ash, percent_adf, percent_ndf, percent_lignin, percent_nfc</td>
<td align="center">real</td>
<td align="center">%</td>
<td>Results from NIR analysis.</td>
</tr>
<tr class="odd">
<td>percent_n_nir, percent_cellulose_calc, percent_hemicellulose_calc</td>
<td align="center">real</td>
<td align="center">%</td>
<td>Calculated results from NIR analysis .</td>
</tr>
<tr class="even">
<td>percent_carb_nnorm, percent_cellulose_nnorm, percent_lignin_nnorm</td>
<td align="center">real</td>
<td align="center">%</td>
<td>Non-normalized results from NIR analysis.</td>
</tr>
<tr class="odd">
<td>percent_carb_norm, percent_cellulose_norm, percent_lignin_norm</td>
<td align="center">real</td>
<td align="center">%</td>
<td>Normalized results from NIR analysis.</td>
</tr>
<tr class="even">
<td>validated_by_creator</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Validation by data creator.</td>
</tr>
<tr class="odd">
<td>validated_by_shepherd</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>Validation by data shepherd.</td>
</tr>
</tbody>
</table>

7. Decomp Biomass
=================

<br> **Description:**

-   The `decomp_biomass_fresh` and `decomp_biomass_dry` tables records cover crop biomass decomposition data collected after cover crop termination. The `decomp_biomass_cn` records cover crop C/N fraction data measured by our on-farm team in the lab. The `decomp_biomass_ash` table records the cover crop ashless weights measured by our on-farm team in the lab. Data in all these tables will be collected using Kobo. There are validation columns in each of these four tables. The field team will be able to suggest edits in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   Rows in the `decomp_biomass_fresh`, `decomp_biomass_dry`, `decomp_biomass_cn`, and `decomp_biomass_ash` tables will be pre-filled with empty values (and zeros in the validation columns) when the *decomp\_biomass* protocol is selected for at onboarding. Rows will be pre-filled with missing values (including values in the validation columns) if the *decomp\_biomass* protocol is not selected at onboarding. 20 rows should be created in each table for each site code.

-   Data validation will be performed for each observation/row in the data table. After validation by the data creator and data shepherd, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 1. If data are validated by both the data creator and the data shepherd while some values are still missing, then all missing values will be converted to -999. If all data are missing for a given sample then all values for that sample and table are converted to -999 and values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that code are set to -999.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_decomp_biomass.png" id="id" class="class" style="width:65.0%" style="height:65.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`subplot`:

| Columns | Data Type | Unit | Description        |
|---------|:---------:|:----:|--------------------|
| subplot |  integer  | *NA* | List all subplots. |

`subsamples`:

| Columns   | Data Type | Unit | Description          |
|-----------|:---------:|:----:|----------------------|
| subsample |  char(1)  | *NA* | List all subsamples. |

`times`:

| Columns | Data Type | Unit | Description                                       |
|---------|:---------:|:----:|---------------------------------------------------|
| time    |  integer  | *NA* | List all numeric/relative decomp bag pickup time. |

`decomp_biomass_fresh`:

| Columns                 | Data Type | Unit | Description                              |
|-------------------------|:---------:|:----:|------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                  |
| code                    |  char(3)  | *NA* | Site code.                               |
| subplot                 |  integer  | *NA* | Subplot in site.                         |
| subsample               |  char(1)  | *NA* | Subsample in subplot.                    |
| time                    |  integer  | *NA* | Numeric/relative decomp bag pickup time. |
| empty\_bag\_wt          |    real   |   g  | Empty bag weight.                        |
| fresh\_biomass\_wt      |    real   |   g  | Fresh biomass plus bag weight.           |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.              |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd.             |

`decomp_biomass_dry`:

| Columns                 | Data Type | Unit | Description                              |
|-------------------------|:---------:|:----:|------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                  |
| code                    |  char(3)  | *NA* | Site code.                               |
| subplot                 |  integer  | *NA* | Subplot in site.                         |
| subsample               |  char(1)  | *NA* | Subsample in subplot.                    |
| time                    |  integer  | *NA* | Numeric/relative decomp bag pickup time. |
| recovery\_date          |    date   | *NA* | Decomp bag recovery date.                |
| dry\_biomass\_wt        |    real   |   g  | Dry biomass plus bag weight.             |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.              |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd.             |

`decomp_biomass_cn`:

| Columns                 | Data Type | Unit | Description                                         |
|-------------------------|:---------:|:----:|-----------------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                             |
| code                    |  char(3)  | *NA* | Site code.                                          |
| subplot                 |  integer  | *NA* | Subplot in site.                                    |
| subsample               |  char(1)  | *NA* | Subsample in subplot.                               |
| time                    |  integer  | *NA* | Numeric/relative decomp bag pickup time.            |
| percent\_c              |    real   |   %  | Percent carbon in decomposing cover crop residue.   |
| percent\_n              |    real   |   %  | Percent nitrogen in decomposing cover crop residue. |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.                         |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd.                        |

`decomp_biomass_ash`:

| Columns                 | Data Type | Unit | Description                                        |
|-------------------------|:---------:|:----:|----------------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                            |
| code                    |  char(3)  | *NA* | Site code.                                         |
| subplot                 |  integer  | *NA* | Subplot in site.                                   |
| subsample               |  char(1)  | *NA* | Subsample in subplot.                              |
| time                    |  integer  | *NA* | Numeric/relative decomp bag pickup time.           |
| crucible\_wt            |    real   |   g  | Weight of crucible fraction in cover crop residue. |
| tot\_bwt\_at\_65        |    real   |   g  | Total biomass weight after drying at 65oC.         |
| tot\_bwt\_at\_550       |    real   |   g  | Total biomass weight after drying at 550oC.        |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.                        |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd.                       |

8. Soil Texture from Samples
============================

<br> **Description:**

-   The `texture_from_sample` table records soil texture measured at the TDR sensors from 0 to 30 cm, 30 to 60 cm, and 60 to 100 cm. Texture will be evaluated by an external laband results will be uploaded into the database via an upload functionality in the tech dashboard. There are two validation columns in this table. Data should not need to be updated after import, provided that the uploaded file is in the proper format.

-   Rows in the `texture_from_samples` table will be pre-filled with empty values (and zeros in the validation columns) when the *soil\_texture* protocol is selected at onboarding. Rows will be pre-filled with missing values (including values in the validation columns) when the *soil\_texture* protocol is not selected at onboarding. 6 rows should be created in each table for each site code.

-   Data validation will be performed for each observation/row in the data table. After validation by the data creator and data shepherd, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 1. If texture data is missing for a given site and subplot then all values are converted to -999 and values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that code are set to -999.

-   Every words of the character strings inputed in the *tclass* column are be capitalized.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_texture_from_samples.png" id="id" class="class" style="width:42.0%" style="height:42.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`subplot`:

| Columns | Data Type | Unit | Description        |
|---------|:---------:|:----:|--------------------|
| subplot |  integer  | *NA* | List all subplots. |

`depths`:

| Columns | Data Type | Unit | Description                           |
|---------|:---------:|:----:|---------------------------------------|
| depth   |  integer  | *NA* | List all soil sampling depths levels. |

`textural_classes`:

| Columns |  Data Type  | Unit | Description                                          |
|---------|:-----------:|:----:|------------------------------------------------------|
| tclass  | charvar(25) | *NA* | List all soil texture classes (USDA classification). |

`texture_from_samples`:

| Columns                 |  Data Type  | Unit | Description                  |
|-------------------------|:-----------:|:----:|------------------------------|
| cid                     |    serial   | *NA* | Unique row id (ignore).      |
| code                    |   char(3)   | *NA* | Site code.                   |
| subplot                 |   integer   | *NA* | Subplot in site.             |
| depth                   |   integer   | *NA* | Depth level.                 |
| sand                    |     real    |   %  | Percent sand in soil.        |
| silt                    |     real    |   %  | Percent silt in soil.        |
| clay                    |     real    |   %  | Percent clay in soil.        |
| tclass                  | charvar(25) | *NA* | Sample texture class.        |
| notes                   |     text    | *NA* | Optional notes.              |
| validated\_by\_creator  |   integer   | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |   integer   | *NA* | Validation by data shepherd. |

9. Cash Crop Yield
==================

<br> **Description:**

-   The `yield_in_field` table records the row spacing and plant population data observed in the field. This data is necessary to calculate final plant population and cash crop yield. Population counts will only be made for corn since cotton and soybean have the ability to compensate for missing plants and lower populations (to a certain extent). Data in this table will be collected using Kobo. The `yield_corn`, `yield_soybeans`, and `yield_cotton` records the yield data collected in the field and measured in the lab. Data in this tables will be collected using Kobo. There are validation columns in these four table. The field team will be able to suggest edits in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   Rows in the `yield_in_field` table will be pre-filled with empty values (and zeros in the validation columns) when the *cash\_crop\_yield* protocol is selected at onboarding. Rows in the `yield_corn`, `yield_soybeans`, and `yield_cotton` tables will be pre-filled accordingly to the specified cash crop. Rows will be pre-filled with missing values (including values in the validation columns) when the *cash\_crop\_yield* protocol is not selected at onboarding.

-   The data flow teams needs to think about the steps to be taken if someone entered the wrong cash crop at onboarding. Then, the pre-filled row will have to be deleted in the yield table corresponding to the former cash crop, and a new row will have to be created in the yield table corresponding to the newly specified cash crop.

-   Data validation will be performed for each observation/row in the data tables. After validation by the data creator and data shepherd, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 1. If the cash crop yield data are validated by both the data creator and the data shepherd while some values are still missing, then all missing values will be converted to -999. If all data is missing for a given site and table then all values are converted to -999 and values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that code are set to -999.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_yield.png" id="id" class="class" style="width:75.0%" style="height:75.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`treatments`:

| Columns   | Data Type | Unit | Description           |
|-----------|:---------:|:----:|-----------------------|
| treatment |  char(1)  | *NA* | Lists all treatments. |

`subplot`:

| Columns | Data Type | Unit | Description        |
|---------|:---------:|:----:|--------------------|
| subplot |  integer  | *NA* | List all subplots. |

`rows`:

| Columns | Data Type | Unit | Description    |
|---------|:---------:|:----:|----------------|
| row     |  char(2)  | *NA* | List all rows. |

`yield_in_field`:

| Columns                 | Data Type | Unit | Description                                         |
|-------------------------|:---------:|:----:|-----------------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                             |
| code                    |  char(3)  | *NA* | Site code.                                          |
| treatment               |  char(1)  | *NA* | Treatment in site.                                  |
| subplot                 |  integer  | *NA* | Subplot in treatment.                               |
| row                     |  char(2)  | *NA* | Row in subplot.                                     |
| row\_spacing            |    real   |  cm  | Cash crop row spacing.                              |
| stand\_count            |    real   | *NA* | Number of plant per 10 ft row length \[corn only\]. |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.                         |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd.                        |

`yield_corn`:

| Columns                 | Data Type |  Unit  | Description                           |
|-------------------------|:---------:|:------:|---------------------------------------|
| cid                     |   serial  |  *NA*  | Unique row id (ignore).               |
| code                    |  char(3)  |  *NA*  | Site code.                            |
| treatment               |  char(1)  |  *NA*  | Treatment in site.                    |
| subplot                 |  integer  |  *NA*  | Subplot in treatment.                 |
| row                     |  char(2)  |  *NA*  | Row in subplot.                       |
| fresh\_harvest\_wt      |    real   |   kg   | Fresh grain/seed weight.              |
| moisture\_1             |    real   |    %   | Test moisture content, measurement 1. |
| moisture\_2             |    real   |    %   | Test moisture content, measurement 2. |
| grain\_test\_1          |    real   |  lb/bu | Grain test weight, measurement 1.     |
| grain\_test\_2          |    real   | lbs/bu | Grain test weight, measurement 2.     |
| notes                   |    text   |  *NA*  | Optional notes.                       |
| validated\_by\_creator  |  integer  |  *NA*  | Validation by data creator.           |
| validated\_by\_shepherd |  integer  |  *NA*  | Validation by data shepherd.          |

`yield_soybeans`:

| Columns                 | Data Type |  Unit  | Description                           |
|-------------------------|:---------:|:------:|---------------------------------------|
| cid                     |   serial  |  *NA*  | Unique row id (ignore).               |
| code                    |  char(3)  |  *NA*  | Site code.                            |
| treatment               |  char(1)  |  *NA*  | Treatment in site.                    |
| subplot                 |  integer  |  *NA*  | Subplot in treatment.                 |
| row                     |  char(2)  |  *NA*  | Row in subplot.                       |
| fresh\_harvest\_wt      |    real   |   kg   | Fresh grain/seed weight.              |
| moisture\_1             |    real   |    %   | Test moisture content, measurement 1. |
| moisture\_2             |    real   |    %   | Test moisture content, measurement 2. |
| grain\_test\_1          |    real   |  lb/bu | Grain test weight, measurement 1.     |
| grain\_test\_2          |    real   | lbs/bu | Grain test weight, measurement 2.     |
| notes                   |    text   |  *NA*  | Optional notes.                       |
| validated\_by\_creator  |  integer  |  *NA*  | Validation by data creator.           |
| validated\_by\_shepherd |  integer  |  *NA*  | Validation by data shepherd.          |

`yield_cotton`:

| Columns                 | Data Type | Unit | Description                  |
|-------------------------|:---------:|:----:|------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).      |
| code                    |  char(3)  | *NA* | Site code.                   |
| treatment               |  char(1)  | *NA* | Treatment in site.           |
| subplot                 |  integer  | *NA* | Subplot in treatment.        |
| row                     |  char(2)  | *NA* | Row in subplot.              |
| boll\_wt                |    real   |   g  | Cotton boll weight.          |
| ginned\_lint\_wt        |    real   |   g  | Cotton ginned lint weight.   |
| notes                   |    text   | *NA* | Optional notes.              |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd. |

10. GPS Locations
=================

<br> **Description:**

-   The `gps_corners` and `gps_corners_validation` tables records the GPS locations of the 4 cover crop and bare treatment corners in each site. The `gps_subplots` and `gps_subplots_validation` tables record the GPS locations of the TDR sensors placed in the cover crop and bare treatments in each site. The information will be gathered using Kobo. GPS data will be visibile on a map from the tech dashboard.After data collection, the field team will be able to suggest edits in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   Validation columns are associated with the `gps_corners` and `gps_subplots` tables. Because the number of points collected might vary between sites, validation of the `gps_corners` and `gps_subplots` tables data is completed in separate tables: `gps_corners_validation` and `gps_subplots_validation`, respectively. Validation will be performed by site code. Only the `gps_corners_validation` and `gps_subplots_validation` tables will be pre-filled at onboarding. By default, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 0 if the *gps\_locations* protocol is selected for particular site. Values in these columns are set to -999 otherwise. If at least one row/observation is recorded in the `gps_corners` and `gps_subplots` tables for a given site and that information was validated by the data creator and shepherd, then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that particular site are set to 1. If data is missing for a particular site, and the missing data is being validated by the data creator and shepherd then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that site are set to -999.

-   The data flow team will need to think about how to manage GPS points that were not being collected properly (update if possible, and delete obsolete coordinates).

<br> **Relational Diagram:**

<br> <img src="Additional/RD_gps_locations.png" id="id" class="class" style="width:85.0%" style="height:85.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`treatments`:

| Columns   | Data Type | Unit | Description           |
|-----------|:---------:|:----:|-----------------------|
| treatment |  char(1)  | *NA* | Lists all treatments. |

`subplot`:

| Columns | Data Type | Unit | Description        |
|---------|:---------:|:----:|--------------------|
| subplot |  integer  | *NA* | List all subplots. |

`gps_corners`:

| Columns   | Data Type |      Unit      | Description                       |
|-----------|:---------:|:--------------:|-----------------------------------|
| cid       |   serial  |      *NA*      | Unique row id (ignore).           |
| code      |  char(3)  |      *NA*      | Site code.                        |
| treatment |  char(1)  |      *NA*      | Treatment in site.                |
| latitude  |    real   | decimal degree | Latitude of GPS point \[WGS84\].  |
| longitude |    real   | decimal degree | Longitude of GPS point \[WGS84\]. |

`gps_corners_validation`:

| Columns                 | Data Type | Unit | Description                  |
|-------------------------|:---------:|:----:|------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).      |
| code                    |  char(3)  | *NA* | Site code.                   |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd. |

`gps_corners`:

| Columns   | Data Type |      Unit      | Description                       |
|-----------|:---------:|:--------------:|-----------------------------------|
| cid       |   serial  |      *NA*      | Unique row id (ignore).           |
| code      |  char(3)  |      *NA*      | Site code.                        |
| treatment |  char(1)  |      *NA*      | Treatment in site.                |
| subplot   |  integer  |      *NA*      | Subplot in treatment.             |
| latitude  |    real   | decimal degree | Latitude of GPS point \[WGS84\].  |
| longitude |    real   | decimal degree | Longitude of GPS point \[WGS84\]. |

`gps_subplots_validation`:

| Columns                 | Data Type | Unit | Description                  |
|-------------------------|:---------:|:----:|------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).      |
| code                    |  char(3)  | *NA* | Site code.                   |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd. |

11. Sensor Data
===============

-   The `water_gateway_data`, `water_node_data`, `water_sensor_data`, and `ambient_sensor_data` tables record the hourly gateway, node, TDR sensor, and humidity plus temperature data, respectively. Data will be imported from hologram. If hologram fails, data will be downloaded and imported from the gateway SD card or node flash memory. Site codes, treatment, and subplot information are not being stored in these table. Allocation to a given site, treatment, and subplots will be made using the `wsensor_install` table. Data in this table will be collected using Kobo. Only the sensor install data will be validated. Because material failure is possible, more than one observation/row can be entered for a given site code and validation will be conducted by site code in a separate table: `wsensor_install_validation`. The `hologram_devices` table lists the active devices in hologram and their unique hologram attributes. The `wsensor_setup` table will record the current device radio id to help address new node and gateways when equipment fails.

-   The field team will be able to suggest edits only for the `wsensor_install` and `wsensor_setup` tables. Edits will be suggested in the tech dashboard and the designated data shepherd will update data as needed. GIT issue tracker will be used in the background to keep track of suggestions and data updates.

-   Only the `wsensor_install_validation` table will be pre-filled during onboarding. Validation will be performed by site code. By default, values in the *validation\_by\_creator* and *validation\_by\_shepherd* columns are set to 0 if the *sensor\_data* protocol is selected for particular site, and -999 otherwise. If at least one row/observation is recorded in the `wsensor_install` table for a given site and that information was validated by the data creator and shepherd, then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that particular site are set to 1. If data is missing for a particular site, and the missing data is being validated by the data creator and shepherd then values of the *validation\_by\_creator* and *validation\_by\_shepherd* columns for that site are set to -999.

-   Values in *ts\_up* columns are *null* if data was not successfully uploaded in hologram and manually collected from the SD card or flash memory. Project ID is `PSA` for PSA on-farm trials. Uppercase values in *tdr\_address* indicate data collected in the bare treatment while lowercase values indicate data collected in the cover crop treatment. Sensor *center\_depth* values are negative. Sensor height values are positive.

-   Additional information about the sensor data inventory can be found here: <https://docs.google.com/spreadsheets/d/1AqQ0j0mfNxKNU8u3s0NMxRO_-8DVzPzMzpEgAvC7Uuk/edit#gid=2136347016>

<br> **Relational Diagram:**

<br> <img src="Additional/RD_parsed_sensor_data.png" id="id" class="class" style="width:80.0%" style="height:80.0%" /> <img src="Additional/RD_sensor_data_flow.png" id="id" class="class" style="width:75.0%" style="height:75.0%" /> <img src="Additional/RD_sensor_repairs.png" id="id" class="class" style="width:40.0%" style="height:40.0%" />

<br>

**Data dictionary by table:**

`water_gateway_data`:

| Columns             |  Data Type |   Unit  | Description                                             |
|---------------------|:----------:|:-------:|---------------------------------------------------------|
| uid                 |   serial   |   *NA*  | Unique row id (ignore).                                 |
| gateway\_serial\_no |   char(8)  |   *NA*  | Gateway serial number, 8-digit barcode.                 |
| timestamp           |  timestamp |   *NA*  | Time at which data were recorded \[GMT\].               |
| ts\_up              |  timestamp |   *NA*  | Time at which data were uploaded into hologram \[GMT\]. |
| firmware\_version   |  char(11)  |   *NA*  | Firmware version.                                       |
| project\_id         | charvar(5) |   *NA*  | Project ID.                                             |
| gw\_batt\_voltage   |    real    |    V    | Gateway battery voltage.                                |
| gw\_enclosure\_temp |    real    | Celcius | Gateway enclosure temperature.                          |
| gw\_solar\_voltage  |    real    |    V    | Gateway photovoltaic voltage.                           |
| gw\_solar\_current  |    real    |    mA   | Gateway photovoltaic current.                           |

`water_node_data`:

| Columns             |  Data Type |   Unit  | Description                                             |
|---------------------|:----------:|:-------:|---------------------------------------------------------|
| uid                 |   serial   |   *NA*  | Unique row id (ignore).                                 |
| node\_serial\_no    |   char(8)  |   *NA*  | Node serial number, 8-digit barcode.                    |
| timestamp           |  timestamp |   *NA*  | Time at which data were recorded \[GMT\].               |
| ts\_up              |  timestamp |   *NA*  | Time at which data were uploaded into hologram \[GMT\]. |
| firmware\_version   |  char(11)  |   *NA*  | Firmware version.                                       |
| project\_id         | charvar(5) |   *NA*  | Project ID.                                             |
| nd\_batt\_voltage   |    real    |    V    | Node battery voltage.                                   |
| nd\_enclosure\_temp |    real    | Celcius | Node enclosure temperature.                             |
| nd\_solar\_voltage  |    real    |    V    | Node photovoltaic voltage.                              |
| nd\_solar\_current  |    real    |    mA   | Node photovoltaic current.                              |
| signal\_strength    |    real    |   dbm   | Signal strength of gateway-node communication.          |

`water_sensor_data`:

| Columns          |  Data Type  |     Unit    | Description                                             |
|------------------|:-----------:|:-----------:|---------------------------------------------------------|
| uid              |    serial   |     *NA*    | Unique row id (ignore).                                 |
| node\_serial\_no |   char(8)   |     *NA*    | Node serial number, 8-digit barcode.                    |
| timestamp        |  timestamp  |     *NA*    | Time at which data were recorded \[GMT\].               |
| ts\_up           |  timestamp  |     *NA*    | Time at which data were uploaded into hologram \[GMT\]. |
| tdr\_sensor\_id  | charvar(33) |     *NA*    | TDR sensor id.                                          |
| center\_depth    |   integer   |      cm     | Depth at the center of the sensor.                      |
| tdr\_address     |   char(1)   |     *NA*    | TDR sensor address.                                     |
| vwc              |     real    |   vol/vol   | Volumetric soil water content.                          |
| soil\_temp       |     real    |   Celcius   | Soil temperature.                                       |
| permittivity     |     real    |     *NA*    | Soil permittivity.                                      |
| ec\_bulk         |     real    | 10^(-6)S/cm | Bulk soil electro-conductivity.                         |
| ec\_pore\_water  |     real    | 10^(-6)S/cm | Pore water soil electro-conductivity.                   |
| travel\_time     |   numeric   |      ps     | Travel time.                                            |

`ambient_sensor_data`:

| Columns          |  Data Type  |   Unit  | Description                                             |
|------------------|:-----------:|:-------:|---------------------------------------------------------|
| uid              |    serial   |   *NA*  | Unique row id (ignore).                                 |
| node\_serial\_no |   char(8)   |   *NA*  | Node serial number, 8-digit barcode.                    |
| timestamp        |  timestamp  |   *NA*  | Time at which data were recorded \[GMT\].               |
| ts\_up           |  timestamp  |   *NA*  | Time at which data were uploaded into hologram \[GMT\]. |
| rh\_sensor\_id   | charvar(33) |   *NA*  | Humidity sensor ID.                                     |
| rh\_address      |   char(1)   |   *NA*  | Humidity sensor address.                                |
| rh\_height       |   integer   |    cm   | Height of the humidity center.                          |
| t\_amb           |     real    | Celcius | Air temperature at the humidity sensor.                 |
| rh               |     real    |    %    | Relative humidity.                                      |
| temp\_sensor\_id | charvar(33) |   *NA*  | Temperature sensor ID.                                  |
| temp\_address    |   char(1)   |   *NA*  | Temperature sensor addres.                              |
| temp\_height     |   integer   |    cm   | Height of the temperature sensor.                       |
| t\_lb            |     real    | Celcius | Temperature below decomp bag.                           |

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`subplot`:

| Columns | Data Type | Unit | Description        |
|---------|:---------:|:----:|--------------------|
| subplot |  integer  | *NA* | List all subplots. |

`wsensor_install`:

| Columns                 | Data Type | Unit | Description                                       |
|-------------------------|:---------:|:----:|---------------------------------------------------|
| cid                     |   serial  | *NA* | Unique row id (ignore).                           |
| code                    |  char(3)  | *NA* | Site code.                                        |
| subplot                 |  integer  | *NA* | Subplot in site.                                  |
| gateway\_serial\_no     |  char(8)  | *NA* | Gateway serial number.                            |
| bare\_node\_serial\_no  |  char(8)  | *NA* | Bare node serial number.                          |
| cover\_node\_serial\_no |  char(8)  | *NA* | Cover crop node serial number.                    |
| time.begin              | timestamp | *NA* | Time when gateway and nodes are paired \[GMT\].   |
| time.end                | timestamp | *NA* | Time when gateway and nodes are unpaired \[GMT\]. |
| notes                   |    text   | *NA* | Optional notes.                                   |

`wsensor_install_validation`:

| Columns                 | Data Type | Unit | Description                  |
|-------------------------|:---------:|:----:|------------------------------|
| uid                     |   serial  | *NA* | Unique row id (ignore).      |
| code                    |  char(3)  | *NA* | Site code.                   |
| subplot                 |  integer  | *NA* | Subplot in site.             |
| validated\_by\_creator  |  integer  | *NA* | Validation by data creator.  |
| validated\_by\_shepherd |  integer  | *NA* | Validation by data shepherd. |

`hologram_metadata`:

| Columns             |  Data Type  | Unit | Description                                           |
|---------------------|:-----------:|:----:|-------------------------------------------------------|
| uid                 |    serial   | *NA* | Unique row id (ignore).                               |
| device\_name        |     text    | *NA* | Device name in hologram.                              |
| device\_id          | numeric(10) | *NA* | Device id in hologram.                                |
| gateway\_serial\_no |   char(8)   | *NA* | Gateway serial number, 8-digit barcode.               |
| time.begin          |  timestamp  | *NA* | Time when gateway and sim card were paired \[GMT\].   |
| time.end            |  timestamp  | *NA* | Time when gateway and sim card were unpaired \[GMT\]. |
| link\_id            | numeric(10) | *NA* | Link id in hologram.                                  |
| org\_id             | numeric(10) | *NA* | Org id in hologram.                                   |
| device\_state       |     bool    | *NA* | Device status in hologram.                            |

`wsensor_device_types`:

| Columns      |  Data Type  | Unit | Description                      |
|--------------|:-----------:|:----:|----------------------------------|
| device\_type | charvar(15) | *NA* | Lists all possible device types. |

`wsensor_setup`:

| Columns      |  Data Type  | Unit | Description                                           |
|--------------|:-----------:|:----:|-------------------------------------------------------|
| uid          |    serial   | *NA* | Unique row id (ignore).                               |
| device\_type | charvar(15) | *NA* | Device types.                                         |
| serial\_no   |   char(8)   | *NA* | Device serial number, 8-digit barcode.                |
| radio\_id    |  charvar(2) | *NA* | Device radio ID.                                      |
| time.begin   |  timestamp  | *NA* | Time when gateway and radio ID were paired \[GMT\].   |
| time.end     |  timestamp  | *NA* | Time when gateway and radio ID were unpaired \[GMT\]. |

12. Weather
===========

<br> **Description:**

-   The `rainfall` and `weather` table record hourly rainfall and atmospheric weather data. Weather data will be imported from our weather API. Data are stored in two separate tables because atmospheric weather data are not being updated by the data source as often as the rainfall data. There are no validation columns associated with the weather data. Data sources are being recorded within the data table to keep track of possible future changes in data source.

-   Hourly weather data for any given site were recorded from August 1st of the previous year to December 31st of the cash crop year.

-   Wind speed can be calculated from the *zonal\_wind* and *meridional\_wind* columns using the following formula: `wind_speed [m/s] = sqrt(zonal_wind_speed^2 + meridional_wind_speed^2)`

-   Relative humidity can be calcuated from the humidity column using the following formula: `relative_humidity (((humidity/18)/(((1-humidity)/28.97) + (humidity/18)))*pressure/1000) /` `(0.61*exp((17.27*air_temperature)/(air_temperature + 237.3)))`

<br> **Relational Diagram:**

<br> <img src="Additional/RD_weather.png" id="id" class="class" style="width:35.0%" style="height:35.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`rainfall`:

| Columns                |  Data Type  | Unit | Description             |
|------------------------|:-----------:|:----:|-------------------------|
| cid                    |    serial   | *NA* | Unique row id (ignore). |
| code                   |   char(3)   | *NA* | Site code.              |
| date                   |     date    | *NA* | Date (YYYY-MM-DD).      |
| timez                  |  timestamp  | *NA* | Timestamp \[GMT\].      |
| rainfall               |     real    |  mm  | Rainfall amount.        |
| data\_source\_rainfall | charvar(10) | *NA* | Rainfall data source.   |

`weather`:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cid</td>
<td align="center">serial</td>
<td align="center"><em>NA</em></td>
<td>Unique row id (ignore).</td>
</tr>
<tr class="even">
<td>code</td>
<td align="center">char(3)</td>
<td align="center"><em>NA</em></td>
<td>Site code.</td>
</tr>
<tr class="odd">
<td>date</td>
<td align="center">date</td>
<td align="center"><em>NA</em></td>
<td>Date (YYYY-MM-DD).</td>
</tr>
<tr class="even">
<td>timez</td>
<td align="center">timestamp</td>
<td align="center"><em>NA</em></td>
<td>Timestamp [GMT].</td>
</tr>
<tr class="odd">
<td>air_temperature</td>
<td align="center">real</td>
<td align="center">Celcius</td>
<td>Air temperature. 2m above ground.</td>
</tr>
<tr class="even">
<td>humidity</td>
<td align="center">real</td>
<td align="center">kg/kg</td>
<td>Specific humidity. 2m above ground.</td>
</tr>
<tr class="odd">
<td>longwave_radiation</td>
<td align="center">real</td>
<td align="center">W/m2</td>
<td>Surface downward longwave radiation.</td>
</tr>
<tr class="even">
<td>shortwave_radiation</td>
<td align="center">real</td>
<td align="center">W/m2</td>
<td>Surface downward shortwave radiation.</td>
</tr>
<tr class="odd">
<td>zonal_wind</td>
<td align="center">real</td>
<td align="center">m/s</td>
<td>Zonal wind. 10m above ground.</td>
</tr>
<tr class="even">
<td>meridional_wind</td>
<td align="center">real</td>
<td align="center">m/s</td>
<td>Meridional wind. 10m above ground.</td>
</tr>
<tr class="odd">
<td>pressure</td>
<td align="center">real</td>
<td align="center">Pa</td>
<td>Surface pressure.</td>
</tr>
<tr class="even">
<td>convective_precipitation</td>
<td align="center">real</td>
<td align="center"><em>NA</em></td>
<td>Fraction of total precipitation that is convective.</td>
</tr>
<tr class="odd">
<td>potential_energy</td>
<td align="center">real</td>
<td align="center">J/kg</td>
<td>Convective available potential energy.</td>
</tr>
<tr class="even">
<td>potential_evaporation</td>
<td align="center">real</td>
<td align="center">kg/m2</td>
<td>Potential evaporation.</td>
</tr>
<tr class="odd">
<td>data_source_atmosphere</td>
<td align="center">charvar(10)</td>
<td align="center"><em>NA</em></td>
<td>Atmospheric weather data source.</td>
</tr>
</tbody>
</table>

13. Weed Research
=================

<br> **Description:**

-   Weed pictures will not be stored in this database and the `weed_research` table was designed as a place holder which can used to facilitate linkage between weed pictures and other data collected at PSA on-farm sites. This table is presently empty. There are no validation columns in this table. Data in the `weed_research` table could be imported automatically with a script that automatically parse through the directories containing weed research pictures.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_weed_research.png" id="id" class="class" style="width:52.0%" style="height:52.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`treatments`:

| Columns   | Data Type | Unit | Description           |
|-----------|:---------:|:----:|-----------------------|
| treatment |  char(3)  | *NA* | Lists all treatments. |

`quadrats`:

| Columns | Data Type | Unit | Description         |
|---------|:---------:|:----:|---------------------|
| quadrat |  char(3)  | *NA* | Lists all quadrats. |

`times`:

| Columns | Data Type | Unit | Description                                     |
|---------|:---------:|:----:|-------------------------------------------------|
| time    |  integer  | *NA* | List all numeric/relative data collection time. |

`weed_research`:

| Columns   |  Data Type  | Unit | Description                            |
|-----------|:-----------:|:----:|----------------------------------------|
| cid       |    serial   | *NA* | Unique row id (ignore).                |
| code      |   char(3)   | *NA* | Site code.                             |
| treatment |   char(1)   | *NA* | Treatment in site.                     |
| quadrat   |   integer   | *NA* | Quadrat in treatment.                  |
| time      |   integer   | *NA* | Numeric/relative data collection time. |
| date      |     date    | *NA* | Picture data collection date.          |
| filename  | charvar(30) | *NA* | Picture filename.                      |

14. Surface Bulk Density \[Retired Protocol\]
=============================================

<br> **Description:**

-   The `bulk_density` table contains surface bulk density data for the 2018 NC and MD sites. Data in this table are not being pre-filled at planting since the *bulk\_density* protocol has been retired. There are no validation columns associated with this table.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_bulk_density.png" id="id" class="class" style="width:60.0%" style="height:60.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`treatments`:

| Columns   | Data Type | Unit | Description           |
|-----------|:---------:|:----:|-----------------------|
| treatment |  char(3)  | *NA* | Lists all treatments. |

`subplots`:

| Columns | Data Type | Unit | Description         |
|---------|:---------:|:----:|---------------------|
| subplot |  integer  | *NA* | Lists all subplots. |

`subsamples`:

| Columns   | Data Type | Unit | Description           |
|-----------|:---------:|:----:|-----------------------|
| subsample |  char(1)  | *NA* | Lists all subsamples. |

`bulk_density`:

| Columns                  | Data Type |                 Unit                | Description                       |
|--------------------------|:---------:|:-----------------------------------:|-----------------------------------|
| cid                      |   serial  |                 *NA*                | Unique row id (ignore).           |
| code                     |  char(3)  |                 *NA*                | Site code.                        |
| treatment                |  char(1)  |                 *NA*                | Treatment in site.                |
| subplot                  |  integer  |                 *NA*                | Subplot in treatment.             |
| subsample                |  char(1)  |                 *NA*                | Subsample in subplot.             |
| bag\_wt                  |    real   |                  g                  | Empty bag weight.                 |
| dry\_soil\_plus\_bag\_wt |     g     | Weight of dry soil sample plus bag. |                                   |
| date                     |    date   |                 *NA*                | Date of bulk density measurement. |

15. Soil Nitrogen \[Retired Protocol\]
======================================

<br> **Description:**

-   The `soil_nitrogen` table contains soil nitrogen data for the 2017 and 2018 NC and MD sites. Data in this table are not being pre-filled at planting since the *soil\_nitrogen* protocol has been retired. There are no validation columns associated with this table.

<br> **Relational Diagram:**

<br> <img src="Additional/RD_soil_nitrogen.png" id="id" class="class" style="width:80.0%" style="height:80.0%" />

<br>

**Data dictionary by table:**

`codes`:

| Columns | Data Type | Unit | Description                    |
|---------|:---------:|:----:|--------------------------------|
| code    |  char(3)  | *NA* | Lists all possible site codes. |

`seasons`:

| Columns | Data Type | Unit | Description                 |
|---------|:---------:|:----:|-----------------------------|
| code    |  char(1)  | *NA* | Lists all possible seasons. |

`treatments`:

| Columns   | Data Type | Unit | Description           |
|-----------|:---------:|:----:|-----------------------|
| treatment |  char(3)  | *NA* | Lists all treatments. |

`subplots`:

| Columns | Data Type | Unit | Description         |
|---------|:---------:|:----:|---------------------|
| subplot |  integer  | *NA* | Lists all subplots. |

`depths`:

| Columns | Data Type | Unit | Description              |
|---------|:---------:|:----:|--------------------------|
| depth   |  integer  | *NA* | Lists all depths levels. |

`soil_nitrogen`:

| Columns                       |  Data Type  |               Unit               | Description                        |
|-------------------------------|:-----------:|:--------------------------------:|------------------------------------|
| cid                           |    serial   |               *NA*               | Unique row id (ignore).            |
| code                          |   char(3)   |               *NA*               | Site code.                         |
| season                        |   char(1)   |               *NA*               | Season.                            |
| treatment                     |   char(1)   |               *NA*               | Treatment in site.                 |
| subplot                       |   integer   |               *NA*               | Subplot in treatment.              |
| depth                         |   integer   |               *NA*               | Depth level in subplot.            |
| empty\_bag\_wt                |     real    |                 g                | Empty bag weight.                  |
| soil\_plus\_bag\_wt           |     real    |                 g                | Fresh soil plus bag weight.        |
| rock\_wt                      |     real    |                 g                | Weight of rocks in sample.         |
| tin\_wt                       |     real    |                 g                | Empty tin weight.                  |
| moist\_soil\_plus\_tin\_wt    |     real    |                 g                | Fresh soil in tin plus tin weight. |
| dry\_soil\_plus\_tin\_wt real |      g      | Dry soil in tin plus tin weight. |                                    |
| n\_vial\_nb                   |  charvar(3) |               *NA*               | Vial number for nitrogen analysis. |
| n\_soil\_dry\_wt              |     real    |                 g                | Dry soil weight in vial.           |
| nh4\_ppm                      | charvar(10) |                ppm               | Concentration of NH4 in sample.    |
| no3\_ppm                      | charvar(10) |                ppm               | Concentration of NO3 in sample.    |

`farm_history` \[retired\]:

<table style="width:58%;">
<colgroup>
<col width="11%" />
<col width="16%" />
<col width="11%" />
<col width="19%" />
</colgroup>
<thead>
<tr class="header">
<th>Columns</th>
<th align="center">Data Type</th>
<th align="center">Unit</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cid</td>
<td align="center">serial</td>
<td align="center"><em>NA</em></td>
<td>Unique row id (ignore).</td>
</tr>
<tr class="even">
<td>code</td>
<td align="center">char(3)</td>
<td align="center"><em>NA</em></td>
<td>Site code.</td>
</tr>
<tr class="odd">
<td>fall_sampling_date</td>
<td align="center">date</td>
<td align="center"><em>NA</em></td>
<td>Fall deep core sampling date.</td>
</tr>
<tr class="even">
<td>spring_sampling_date</td>
<td align="center"><em>NA</em></td>
<td align="center">Spring deep core sampling date.</td>
<td></td>
</tr>
<tr class="odd">
<td>post_harvest_fertility</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>1 if post harvest fertility was applied, 0 otherwise.</td>
</tr>
<tr class="even">
<td>post_harvest_fertility_date</td>
<td align="center">date</td>
<td align="center"><em>NA</em></td>
<td>Post-harvest fertility date.</td>
</tr>
<tr class="odd">
<td>post_harvest_fertility_source</td>
<td align="center">text</td>
<td align="center"><em>NA</em></td>
<td>Post-harvest fertility source.</td>
</tr>
<tr class="even">
<td>post_harvest_fertility_rate</td>
<td align="center">text</td>
<td align="center"><em>NA</em></td>
<td>Post-harvest fertility rate.</td>
</tr>
<tr class="odd">
<td>at_pre_planting_fertilization</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>1 if at/pre-planting fertilization was applied.</td>
</tr>
<tr class="even">
<td>at_pre_planting_fertilization_date</td>
<td align="center">date</td>
<td align="center"><em>NA</em></td>
<td>At/pre-planting fertilization date.</td>
</tr>
<tr class="odd">
<td>at_pre_planting_fertilization_method</td>
<td align="center">text</td>
<td align="center"><em>NA</em></td>
<td>At/pre-planting fertilization method.</td>
</tr>
<tr class="even">
<td>at_pre_planting_fertilization_rate</td>
<td align="center">text</td>
<td align="center"><em>NA</em></td>
<td>At/pre-planting fertilization rate.</td>
</tr>
<tr class="odd">
<td>sidedress</td>
<td align="center">integer</td>
<td align="center"><em>NA</em></td>
<td>1 if cash crop was applied, 0 otherwise.</td>
</tr>
<tr class="even">
<td>sidedress_date</td>
<td align="center">date</td>
<td align="center"><em>NA</em></td>
<td>Sidedress date.</td>
</tr>
<tr class="odd">
<td>sidedress_rate</td>
<td align="center">text</td>
<td align="center"><em>NA</em></td>
<td>Sidedress rate.</td>
</tr>
</tbody>
</table>
