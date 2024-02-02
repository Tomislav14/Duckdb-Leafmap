# DuckDB i Leafmap 

1. **Instalacija DuckDB i LeafMap:**
   - Koristite naredbu `%pip install duckdb leafmap` kako biste instalirali potrebne pakete.

2. **Uvoz Biblioteka:**
   - Nakon instalacije, uvezite potrebne biblioteke u Jupyter bilježnicu.
     ```python
     import duckdb
     import leafmap
     ```

3. **Povezivanje s DuckDB Bazom Podataka:**
   - Stvorite vezu s DuckDB bazom podataka koristeći sljedeću naredbu:
     ```python
     con = duckdb.connect("zagreb.db")
     ```

4. **Učitavanje Prostornih Podataka u DuckDB:**
   - Instalirajte i učitajte spatijalnu ekstenziju u DuckDB:
     ```python
     con.install_extension("spatial")
     con.load_extension("spatial")
     ```
   - Zatim stvorite tablicu i učitajte prostorne podatke iz GeoJSON datoteke (primjer):
     ```python
     con.sql('CREATE TABLE IF NOT EXISTS stajalista AS SELECT * FROM ST_Read(\'E:\\duck db shapefileovi\\tramvajska_stajalista_ZET.geojson\')')
     ```

5. **Prikaz Podataka i Analiza:**
   - Pregledajte sadržaj tablice pomoću:
     ```python
     con.table('stajalista').show()
     ```
   - Izvršite SQL upit za stvaranje nove kolone "geometry" s tekstualnim prikazom geometrije:
     ```python
     stajalista_df = con.sql("SELECT * EXCLUDE geom, ST_AsText(geom) as geometry FROM stajalista").df()
     ```

6. **Pretvorba DataFrame-a u GeoDataFrame i Vizualizacija:**
   - Pretvorite DataFrame u GeoDataFrame i prikažite geoprostorne podatke:
     ```python
     stajalista_gdf = leafmap.df_to_gdf(stajalista_df, geometry='geometry', src_crs="EPSG:4326", dst_crs="EPSG:4326")
     stajalista_gdf.explore()
     ```

Ovaj projekt omogućuje korisnicima rad s geoprostornim podacima u DuckDB bazi podataka i vizualizaciju tih podataka pomoću LeafMap biblioteke.

