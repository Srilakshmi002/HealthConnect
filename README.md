# HealthConnect Database Design

[![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)

## Overview

HealthConnect is a relational database design for an integrated healthcare platform that connects patients with providers, facilities, and wellness programs. It supports personalized experiences through appointment scheduling, patient profiles, wellness tracking, and insurance integration. Built with MySQL, the schema is normalized (3NF) with primary/foreign keys, constraints, and modularity for scalability.

- **Technologies**: MySQL, SQL (DDL for schema, DML for test data).
- **Goals**: Enable secure data management for healthcare operations, with potential for analytics and reporting.
- **Key Features**: Geographical hierarchy, provider affiliations, premium memberships, wellness enrollment, and financial tracking.

## Business Context

HealthConnect aims to streamline healthcare access by providing a unified platform for patients and providers. Key requirements include:

### Geographical Hierarchy
- Supports states, cities, and facilities.
- Handles city name disambiguation across states.

### Healthcare Facility Profiles
- Stores names, addresses, ratings, contacts, operating hours.
- Includes appointment links and special services.

### Healthcare Providers
- Records qualifications, specialties, facility affiliations, availability.

### Patient Membership
- Differentiates regular and premium members.
- Stores personal info, medical history, preferences, appointment history.

### Appointment System
- Scheduling, ratings, reviews, follow-ups.

### Premium Patient Features
- Priority booking, telehealth, personalized plans.
- Tracks consultations and health goals.

### Wellness Programs
- Enrollment, progress tracking, feedback.

### Patient Health Journal
- Secure journaling with unique identifiers for privacy.

### Wellness Center Management
- Profiles with services, staff, engagement stats.

### Insurance & Financials
- Links policies, manages billing and payments.

### Community Features
- Sharing health experiences and peer support.

## Database Schema

The schema includes tables for core entities with relationships enforced via foreign keys. Here's a summary:

| Table Name          | Description                          | Key Columns                          |
|---------------------|--------------------------------------|--------------------------------------|
| States             | Geographical states                  | StateID (PK), StateName             |
| Cities             | Cities within states                 | CityID (PK), CityName, StateID (FK) |
| Facilities         | Healthcare facilities                | FacilityID (PK), Name, Address, Rating |
| Providers          | Doctors/specialists                  | ProviderID (PK), Name, Specialty, FacilityID (FK) |
| Patients           | User profiles                        | PatientID (PK), Name, MembershipType |
| Appointments       | Scheduling records                   | AppointmentID (PK), PatientID (FK), ProviderID (FK) |
| WellnessPrograms   | Program details                      | ProgramID (PK), Name, Description   |
| Insurance          | Policies and billing                 | PolicyID (PK), PatientID (FK), Provider |
| ... (additional)   | E.g., Journals, Reviews, Payments    | Varies                               |

- **Normalization**: Achieved 3NF to reduce redundancy (e.g., separate tables for cities/states).
- **Constraints**: Unique keys for identifiers, check constraints for ratings (1-5), cascading deletes where appropriate.

For full details, see [Health Connect DDL.sql](Health Connect DDL.sql).

### ER Diagram and Relational Schema

![ER Diagram](HealthConnectERDiagram.png)  
(Visualizes entities and relationships, e.g., one-to-many between Providers and Facilities.)

![Relational Schema](RelationalSchema.jpg)  
(Shows tables, columns, and keys.)

## Installation and Setup

1. Install MySQL (version 8.0+ recommended).
2. Create a new database: `CREATE DATABASE HealthConnect;`.
3. Run the DDL script: `mysql -u youruser -p HealthConnect < "Health Connect DDL.sql"`.
4. Insert test data: `mysql -u youruser -p HealthConnect < TestDataInsertion.sql`.
5. For phase 3 additions: `mysql -u youruser -p HealthConnect < phase3.sql`.

## Usage

1. Connect to the database: Use MySQL Workbench or command line.
2. Example Query: Retrieve patient appointments  
   ```sql
   SELECT p.Name, a.Date, pr.Name AS Provider
   FROM Patients p
   JOIN Appointments a ON p.PatientID = a.PatientID
   JOIN Providers pr ON a.ProviderID = pr.ProviderID;
