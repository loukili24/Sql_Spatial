# TD-1: Installing PostGIS and Creating a Spatial Database

### SIG-3 | Academic Year: 2024-2025  
**Prof. Hatim Lechgar**

---

## Step 1: Download and Install PostgreSQL

1. Go to the [PostgreSQL download page](https://www.postgresql.org/download/) to get the latest version compatible with your operating system (Windows, MacOS, Linux, etc.).
2. Follow the installation guide:
   - During the installation, set a password for the `postgres` user. Make sure to remember this password for future connections to PostGIS.

---

## Step 2: Download and Install PostGIS

1. During the PostgreSQL installation, check **all components**, including **Stack Builder**.
2. Once the installation is complete, Stack Builder will launch automatically:
   - Select the PostgreSQL version installed on your system.
   - Download **PostGIS**, the spatial database extension for PostgreSQL.
3. After installation, verify that PostGIS has been added.

---

## Step 3: Create a Database with PgAdmin 4

1. Open **PgAdmin 4** and log in using the password set during PostgreSQL installation.
2. In the interface:
   - Expand the **Servers** tree, right-click on **PostgreSQL**, and select:  
     `Create > Database`. 
   - Name the database, e.g., `spatial-db-1`, and click **Save**.
3. Enable the PostGIS extension:
   - Open the **Query Tool** for your new database.
   - Run the following SQL command:
     ```sql
     CREATE EXTENSION postgis;
     ```
   - Press F5 to execute the command.

---

## Verification of Installation

1. Once the command is successfully executed:
   - Expand the newly created database (`spatial-db-1`).
   - Navigate to **Schemas > Public > Tables**.
   - Confirm the presence of the `spatial_ref_sys` table, which verifies that PostGIS is installed correctly.

---

> Congratulations! You have installed PostgreSQL, added the PostGIS extension, and created a spatial database ready for use.

---
