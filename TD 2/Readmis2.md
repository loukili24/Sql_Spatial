# TD-2: Creating Tables with Spatial Columns

### SIG-3 | Academic Year: 2024-2025  
**Prof. Hatim Lechgar**

---

## Step 1: Create a Spatial Table (Points)

### 1. Create the Table
```sql
CREATE TABLE points_of_interests (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    geom GEOMETRY(Point, 4326)
);
```

### 2. Insert Data Using QGIS
Open QGIS and connect to PostgreSQL:
- Right-click on PostgreSQL in the explorer panel.
- Create a **new connection** and enter the required information.
- Test the connection.

Add a layer:
- Double-click the `points_of_interests` table.
- Switch to **edit mode**, add points, and save.

### 3. Verify with PgAdmin
```sql
SELECT * FROM points_of_interests;
```

---

## Step 2: Create a Spatial Table (Polygons)

### 1. Create the Table
```sql
CREATE TABLE protected_areas (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    geom GEOMETRY(Polygon, 4326)
);
```

### 2. Insert Data Using QGIS
Follow the same procedure as for points.

### 3. Verify with PgAdmin
```sql
SELECT * FROM protected_areas;
```

---

## Step 3: Create a Spatial Table (LineStrings)

### 1. Create the Table
```sql
CREATE TABLE routes (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    geom GEOMETRY(LineString, 4326)
);
```

### 2. Insert Data and Verify
Add lines in QGIS as before.

### 3. Verify with PgAdmin
```sql
SELECT * FROM routes;
```

---

## Final Verification
- Check the structure of the created tables in PgAdmin.
- Confirm the presence of geometry columns and added data.

---

> Congratulations! You have successfully created and manipulated spatial tables using PostgreSQL/PostGIS and QGIS.

