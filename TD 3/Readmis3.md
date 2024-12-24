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

---

### 3. Risk zone within a 1 km radius
#### In QGIS:
1. Duplicate the `parcel` layer and name it `fire-risk`.
2. Add a spatial filter to the layer with the following expression:
   ```sql
   ST_DWithin(
     geom, 
     (SELECT ST_Centroid(geom) FROM "public"."parcels" WHERE gid = 462273), 
     1000
   )
   ```
3. Change the symbology to visualize the risk zones.

4. Add a second fire point (parcel `gid = 460957`):
   - Modify the filter to include both zones:
     ```sql
     ST_DWithin(
       geom, 
       (SELECT ST_Centroid(geom) FROM "public"."parcels" WHERE gid IN (462273, 460957)), 
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
     (SELECT ST_Centroid(geom) FROM "public"."parcels" WHERE gid IN (460957, 462273)), 
     1000
   );
   ```

2. **What is the total area of parcels near the fire?**
   ```sql
   SELECT SUM(ST_Area(geom))
   FROM parcels
   WHERE ST_DWithin(
     geom, 
     (SELECT ST_Centroid(geom) FROM "public"."parcels" WHERE gid IN (460957, 462273)), 
     1000
   );
   ```

---

## Expected Results
- Identification of parcels at risk within a 1 km radius.
- Calculation of spatial metrics to assess the fire's impact.

---

