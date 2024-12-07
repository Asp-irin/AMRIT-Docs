# Data export

#### Reporting Database

The **AMRIT Platform** includes a dedicated **Reporting Database(db\_reporting)** designed to handle all reporting-related data. This separation ensures that operational data and reporting queries do not interfere with each other, enhancing performance and reliability for both real-time operations and analytical tasks.

#### Replication Database for High-Load Queries

For executing high-load queries for reporting or analytics, the **AMRIT Platform** recommends setting up a **Replication Database**. This database replicates the main transactional database and serves as a read-only source for heavy queries. By offloading such queries to the replicated database, the main transactional database can focus on application read/write operations without performance degradation.

***

For a detailed explanation of tables, relationships, and database schemas across different service lines (HWC, Scheduler, Inventory, etc.), refer to the [AMRIT Database Schema Documentation.](https://pmp.piramalswasthya.org/confluence/display/AMRIT/AMRIT+Database+Documents)
