# Veterinary-Clinic-Database-Oracle-
HND 252 Data Management 2 CourseWork


## Table of Contents
1. [Overview](#overview)  
2. [Schema](#schema)  
3. [Prerequisites](#prerequisites)  
4. [Setup](#setup)  
5. [Running selective parts](#running-selective-parts)  
6. [Sample validation queries](#sample-validation-queries)  
7. [PL/SQL utilities included](#plsql-utilities-included)  
8. [Roles, users & security](#roles-users--security)  
9. [Repository layout](#repository-layout)  
10. [Team](#team)  
11. [License](#license)

---

## Overview
This repository contains an Oracle SQL script that provisions a complete mini–information system for a veterinary clinic:

- Core tables: `Owners`, `Pets`, `Veterinarians`, `Appointments`, `Treatments`, `Medicines`, `Payments`, `Prescriptions`.
- Sample data for realistic test runs.
- Reporting/analytics queries (commented for reference).
- PL/SQL helper procedures and cursor demos.
- Roles and demo users to illustrate privilege separation.

> **Tip:** Adjust usernames/passwords and comment out the security section when running on shared infrastructure.

## Schema
- **Owners** ↔ **Pets**: one-to-many via `owner_id`.
- **Pets** ↔ **Appointments**: one-to-many via `pet_id`.
- **Veterinarians** ↔ **Appointments**: one-to-many via `vet_id`.
- **Appointments** ↔ **Treatments** / **Payments** / **Prescriptions**: one-to-many via `appointment_id`.
- **Medicines** ↔ **Prescriptions**: many-to-one via `medicine_id`.

All monetary/stock columns include `CHECK` constraints to prevent negatives; `Appointments.status` and `Payments.payment_method` use `CHECK` enums.

## Prerequisites
- Oracle Database 12c or newer (tested with 19c).
- A privileged account that can create users and roles (e.g., `SYS` AS SYSDBA) **or** comment out the security section.
- SQL*Plus or SQLcl to run `@vet_clinic.sql`.

## Setup
1. Clone or download the repository.
2. (Optional) place the coursework PDF under `docs/Data Management Coursework Group 6.pdf`.  
   - Yes, it’s fine to keep the PDF in the repo; if the file is large, consider Git LFS.
3. Connect as a privileged user.
4. Execute:
   ```sql
   @vet_clinic.sql
   ```
5. Verify objects with the validation queries below.

## Running selective parts
- **Only schema & seed data:** run until the comment `--3. Select Statements --`.
- **Skip users/roles on shared DB:** comment out section `--5. Creating User Roles and Granting Privileges`.
- **Rerun from scratch:** drop users/roles or use a fresh schema before re-executing.

## Sample validation queries
- Pets older than 3 years:
  ```sql
  SELECT o.owner_name, p.pet_name, p.species, p.age
  FROM Owners o JOIN Pets p USING (owner_id)
  WHERE p.age > 3
  ORDER BY p.age DESC;
  ```
- Scheduled/Completed appointments:
  ```sql
  SELECT p.pet_name, v.vet_name, a.appointment_date, a.status
  FROM Appointments a
  JOIN Pets p USING (pet_id)
  JOIN Veterinarians v USING (vet_id)
  WHERE a.status IN ('Scheduled','Completed')
  ORDER BY a.appointment_date;
  ```

## PL/SQL utilities included
- `Get_Treatment_Cost(p_appointment_id, p_total_cost OUT)`
- `Check_Payment_Category(p_amount, p_category OUT)`
- `Get_Pet_Age_Category(p_pet_id, p_category OUT)`
- Cursor demos to list pets, appointments, and cost categories.

## Roles, users & security
Roles: `admin_role`, `vet_role`, `receptionist_role`  
Demo users: `admin`, `vet_user`, `receptionist` (passwords in script — change in production).  
Grants illustrate principle of least privilege for clinicians vs. reception.

## Repository layout
- `vet_clinic.sql` — full DDL, DML, queries, PL/SQL, roles/users.
- `docs/Data Management Coursework Group 6.pdf` — coursework PDF (optional; add to repo if license permits).
- `README.md` — this file.

## Team
- COHNDSE252F-010 – M.R.M. Nazeef  
- COHNDSE252F-041 – D.W.M. Lakmal  
- COHNDSE252F-061 – H.A.O.M. Alwis  
- COHNDSE252F-028 – M.L.D. Dananjaya  
- COHNDSE252F-038 – W.W.N.T. Fernando  
- COHNDSE252F-056 – W.Y.D. Fernando  
- COHNDSE252F-057 – I.V.N.S. Ilukpitiya  
- COHNDSE252F-001 – K.A.D.C. Ravindu

## License
Educational use. If publishing publicly, add a formal license (e.g., MIT or CC-BY) that your institution allows.
