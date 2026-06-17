# oracle-dba-ecommerce-schema

## Oracle DBA Project – E-commerce Physical Schema, Security, Audit & Recovery

This repository contains an academic Oracle DBA project for an e-commerce application implemented in Oracle Database using a separate application schema named **ECOM_APP**.

The project covers physical schema design, object distribution across dedicated tablespaces, security configuration using roles and users, auditing of sensitive operations, full RMAN database backup, and a recovery scenario after the loss of a datafile.

---

## Author

- **Paun Marius**
- Project: **Oracle DBA – E-commerce Application**
- Oracle Schema: **ECOM_APP**
- Area: **Database Administration / Oracle DBA**

---

## Project Objective

The objective of this project is to design and implement an Oracle Database structure for an e-commerce application, with a strong focus on physical database administration, security, auditing, backup, and recovery.

The project demonstrates practical Oracle DBA concepts such as:

- creating dedicated tablespaces;
- distributing datafiles across different Linux directories;
- creating a separate application schema;
- defining tables, sequences, constraints, and indexes;
- using LOB/SecureFile storage for product and category descriptions;
- validating physical segments through `DBA_SEGMENTS`;
- configuring roles and privileges;
- testing access for different user roles;
- auditing sensitive operations;
- performing a full RMAN backup;
- restoring and recovering a deleted datafile;
- analyzing SQL processing and query optimization.

---

## Application Structure

The e-commerce application is modeled using the following main components:

### 1. Users

- table: `USERS`;
- tablespace: `TBS_ECOM_USERS`;
- stores account-related information such as username, email, password, role, and creation date.

### 2. Product Catalog

- tables: `CATEGORIES`, `PRODUCTS`;
- tablespace: `TBS_ECOM_CATALOG`;
- stores product categories, products, prices, stock quantities, and CLOB descriptions.

### 3. Shopping Cart

- table: `CART_ITEMS`;
- tablespace: `TBS_ECOM_CART`;
- manages products temporarily added to the shopping cart.

### 4. Orders and Transactions

- tables: `ORDERS`, `ORDER_ITEMS`, `INVOICES`;
- tablespace: `TBS_ECOM_ORDERS`;
- manages customer orders, ordered products, and invoices.

### 5. Indexes

- tablespace: `TBS_ECOM_IDX`;
- stores additional indexes used for joins and filtering operations.

### 6. Temporary Operations

- tablespace: `TBS_ECOM_TEMP`;
- used by Oracle for temporary operations such as sorting, hash joins, and group by operations.

---

## Objects Created in the ECOM_APP Schema

The schema contains the following main objects:

- 7 tables:
  - `USERS`
  - `CATEGORIES`
  - `PRODUCTS`
  - `CART_ITEMS`
  - `ORDERS`
  - `ORDER_ITEMS`
  - `INVOICES`

- 7 sequences for ID generation:
  - `SEQ_USERS_ID`
  - `SEQ_CATEGORIES_ID`
  - `SEQ_PRODUCTS_ID`
  - `SEQ_CART_ITEMS_ID`
  - `SEQ_ORDERS_ID`
  - `SEQ_ORDER_ITEMS_ID`
  - `SEQ_INVOICES_ID`

- constraints:
  - Primary Key;
  - Foreign Key;
  - Unique constraints;
  - Check constraints.

- additional indexes for:
  - products;
  - orders;
  - order items;
  - cart items.

- LOB segments for product and category descriptions.

---

## Security Component

For the security part, several roles and test users were created.

### Oracle Roles

- `ROLE_ECOM_CUSTOMER`
- `ROLE_ECOM_MANAGER`
- `ROLE_ECOM_ADMIN`

### Test Users

- `ECOM_CUSTOMER_U`
- `ECOM_MANAGER_U`
- `ECOM_ADMIN_U`

### Tested Scenarios

- the customer can view the product catalog and update the shopping cart;
- the customer cannot delete products;
- the manager can view users and update product stock or order status;
- the manager cannot delete users;
- the administrator has full access to the application tables.

Negative tests confirmed access restrictions through the Oracle error:

```text
ORA-01031: insufficient privileges
```

---

## Audit Component

Auditing was configured for the `ECOM_MANAGER_U` user because this user can modify sensitive business data such as product prices, stock quantities, and order statuses.

Audited operations include:

- session/logon activity;
- update operations on the `PRODUCTS` table;
- update operations on the `ORDERS` table;
- sensitive insert, update, and delete operations on products.

Audit results are verified using:

```text
DBA_AUDIT_TRAIL
```

---

## Backup & Recovery Component

Oracle RMAN was used for backup and recovery operations.

### Backup Strategy

- Full database backup;
- Current control file backup;
- Backup files stored in:

```text
/ora/backup/ecom
```

### RPO/RTO Indicators

- **RPO:** maximum 24 hours;
- **RTO:** maximum 1 hour.

### Recovery Scenario

A failure scenario was simulated by accidentally deleting the following datafile:

```text
/ora/disk1/ecom_orders01.dbf
```

Recovery was performed using RMAN commands such as:

```text
RESTORE DATAFILE
RECOVER DATAFILE
ALTER DATABASE OPEN
```

After the recovery process, the transactional tables `ORDERS`, `ORDER_ITEMS`, and `INVOICES` were checked to confirm that the data was accessible again.

---

## SQL Processing & Query Optimization Component

The project also includes Oracle SQL optimization exercises focused on query processing and execution plan analysis.

The following topics were covered:

- hard parse vs soft parse;
- use of bind variables;
- `EXPLAIN PLAN`;
- `DBMS_XPLAN.DISPLAY_CURSOR`;
- estimated rows vs actual rows;
- impact of statistics without histograms;
- creating histograms for the `STATUS` column;
- function-based indexes;
- covering indexes;
- invisible indexes;
- rewriting a slow query;
- reducing execution time and logical reads.

---

## Oracle Techniques Used

The project uses the following Oracle concepts and techniques:

- Oracle tablespaces;
- datafiles;
- temporary tablespaces;
- separate application schema;
- sequence objects;
- primary keys;
- foreign keys;
- unique constraints;
- check constraints;
- LOB/SecureFile storage;
- additional indexes;
- role-based access control;
- Oracle auditing;
- RMAN backup;
- RMAN restore and recovery;
- SQL execution plans;
- query tuning;
- histograms;
- bind variables;
- function-based indexes;
- invisible indexes.

---

## Recommended Repository Structure

```text
oracle-dba-ecommerce-schema/
│
├── README.md
├── assessment-1/
│   ├── create_tablespaces.sql
│   ├── create_schema.sql
│   ├── create_tables.sql
│   ├── create_indexes.sql
│   └── verification_queries.sql
│
├── assessment-3/
│   ├── security_roles.sql
│   ├── audit_config.sql
│   ├── backup_ecom.rman
│   └── recovery_scenario.md
│
├── optimization-lab/
│   ├── lab3_sql_processing.sql
│   ├── execution_plans.md
│   └── tuning_notes.md
│
├── docs/
│   └── Oracle_DBA_Assessment1_si_3_Comenzi_Output.pdf
│
└── screenshots/
    └── outputs/
```

---

## Results

The implementation validates the following outcomes:

- the `ECOM_APP` schema was created separately from `SYS` and `SYSTEM`;
- dedicated tablespaces were created for users, catalog, orders, cart, indexes, and temporary operations;
- datafiles were distributed across different Linux directories;
- tables, sequences, constraints, indexes, and LOB segments were created;
- physical segments were confirmed through `DBA_SEGMENTS`;
- roles and test users were created with different privilege levels;
- positive and negative tests confirmed role-based access control;
- audit records captured manager operations;
- RMAN backup was completed successfully;
- the recovery scenario restored the deleted datafile;
- transactional data was accessible after recovery;
- SQL optimization exercises demonstrated the impact of statistics, histograms, and indexes on execution plans.

---

## Project Status

This project was developed for academic purposes as part of a **Database Administration** course, using an e-commerce application as the database scenario.

---

## Notes

This project focuses on Oracle DBA concepts and does not represent a complete commercial application.

The following elements are not included:

- frontend application;
- backend API;
- real payment processing;
- real user data;
- integration with external services;
- production infrastructure.

The project demonstrates practical skills in Oracle Database administration, security, auditing, backup/recovery, and SQL query optimization.
