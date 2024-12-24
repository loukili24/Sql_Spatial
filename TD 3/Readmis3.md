# TD-3: Spatial SQL Functions

**Year:** 2024-2025  
**Professor:** Pr. Hatim Lechgar  
**Course:** GIS-3  

---

## Steps

### 1. Display a sample of the data
```sql
SELECT * FROM parcels LIMIT 5;
```

---

### 2. Identify the fire point
Select the centroid of the parcel with `gid = 462273`:
```sql
SELECT ST_Centroid(geom) AS fire_point
FROM parcels
WHERE gid = 462273;
```

Count the parcels within a 1 km radius of the fire point:
```sql
SELECT COUNT(*)
FROM parcels
WHERE ST_DWithin(
    geom,
    (SELECT ST_Centroid(geom) FROM parcels WHERE gid = 462273),
    1000
);
```

---

### 3. Risk zone within a 1 km radius
#### In QGIS:
1. Duplicate the `parcel` layer and name it `fire-risk`.
2. Add a spatial filter to the layer with the following expression:
   ```sql
   ST_DWithin(
     geom, 
     (SELECT ST_Centroid(geom) FROM parcels WHERE gid = 462273), 
     1000
   )
   ```
3. Count parcels within 1 km of two fire points:
   ```sql
   SELECT COUNT(*)
   FROM parcels
   WHERE ST_DWithin(
       geom,
       (SELECT ST_Centroid(geom) FROM parcels WHERE gid = 462273),
       1000
   )
   OR ST_DWithin(
       geom,
       (SELECT ST_Centroid(geom) FROM parcels WHERE gid = 460957),
       1000
   );
   ```
4. Change the symbology to visualize the risk zones.

5. Add a second fire point (parcel `gid = 460957`) and modify the filter to include both zones:
   ```sql
   ST_DWithin(
       geom, 
       (SELECT ST_Centroid(geom) FROM parcels WHERE gid IN (462273, 460957)), 
       1000
   )
   ```

---

### 4. Additional questions
1. **How many parcels are within a 1 km radius of both fire points?**
   ```sql
   SELECT COUNT(*)
   FROM parcels
   WHERE ST_DWithin(
       geom, 
       (SELECT ST_Centroid(geom) FROM parcels WHERE gid IN (460957, 462273)), 
       1000
   );
   ```

2. **What is the total area of parcels near the fire?**
   ```sql
   SELECT SUM(ST_Area(geom))
   FROM parcels
   WHERE ST_DWithin(
       geom, 
       (SELECT ST_Centroid(geom) FROM parcels WHERE gid IN (460957, 462273)), 
       1000
   );
   ```

For an alternative approach:
```sql
SELECT SUM(area)
FROM (
    SELECT ST_Area(geom) AS area
    FROM parcels
    WHERE ST_DWithin(
        geom,
        (SELECT ST_Centroid(geom) FROM parcels WHERE gid = 462273),
        1000
    )
    OR ST_DWithin(
        geom,
        (SELECT ST_Centroid(geom) FROM parcels WHERE gid = 460957),
        1000
    )
) AS subquery;
```

---

## SQL Commands for Spatial Analysis

### Check Indexes on the `parcels` Table
Retrieve the list of indexes defined on the `parcels` table:
```sql
SELECT *
FROM pg_indexes
WHERE tablename = 'parcels';
```

---

### Find Overlapping Geometries Between Parcels
Identify overlapping geometries between parcels:
```sql
SELECT p1.geom, p2.geom
FROM parcels AS p1, parcels AS p2
WHERE p1.geom && p2.geom 
  AND p1.gid != p2.gid 
  AND ST_Overlaps(p1.geom, p2.geom) = TRUE;
```

---

### Create a GiST Index on the `geom` Column
Improve spatial queries by creating a GiST index:
```sql
CREATE INDEX parcels_gist_idx 
ON public.parcels
USING gist (geom);
```

---

### Re-run Overlapping Geometries Query (with GiST Index)
Use the GiST index to enhance performance when finding overlapping geometries:
```sql
SELECT *
FROM parcels AS p1, parcels AS p2
WHERE p1.gid != p2.gid 
  AND ST_Overlaps(p1.geom, p2.geom) = TRUE;
```

---

## Notes
- The `&&` operator in PostGIS checks for bounding box overlap before evaluating the actual geometry overlap.
- The `ST_Overlaps` function determines if two geometries share some, but not all, of the same space.
- Creating a `GiST` index can significantly improve the performance of spatial queries.

---



