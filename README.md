# Awesome Geo Resources

> **Reference material for the geospatial ecosystem — algorithms, libraries, databases, and service APIs.**

---

## 1. Algorithms & Analysis

### Distance & Earth Models

| Algorithm | Approach | Accuracy | Speed | Use Case |
|-----------|----------|----------|-------|----------|
| **Haversine** | Great-circle on sphere | ~0.3% error | Fast | Quick distance estimates |
| **Vincenty** | Iterative on WGS-84 ellipsoid | < 0.5mm | Slow | Precision geodesy |
| **Karney** | Series on ellipsoid (GeographicLib) | < 15nm | Fast | Modern replacement for Vincenty |

### Point & Polygon Analysis

| Algorithm | Description | Common Use |
|-----------|-------------|------------|
| **Ray-Casting** | Point-in-polygon via ray intersection count | Spatial queries, click detection |
| **Convex Hull** | Smallest enclosing convex polygon | Bounding area, outlier detection |
| **Winding Number** | Point-in-polygon via angle summation | Handles self-intersecting polygons |
| **Ear Clipping** | Polygon triangulation by ear removal | Mesh generation, rendering |

### 2D Perspective & Visibility Analysis

| Analysis | Input Data | Description | Key Sources |
|----------|------------|-------------|-------------|
| **Viewshed** | DSM/DTM | Visible areas from observer point(s) | SRTM, Copernicus DEM, LiDAR |
| **Line of Sight** | DSM + obstacles | Binary visible/hidden check between two points | OSM building heights, LiDAR |
| **Topographic Profile** | DEM | 2D cross-section along a path | USGS 3DEP, Mapzen tiles |
| **Aspect & Slope** | DEM | Surface orientation and steepness | GDAL (`gdaldem`), SRTM |
| **Shadow & Solar** | DSM + ephemeris | Shadow casting over time | SunCalc API, Overture Maps |
| **Skyline/Prominence** | DEM + peaks | Feature visibility against sky | Peakbagger, OSM peaks |
| **Sky-View Factor** | DSM | Proportion of sky visible from ground | Urban canyon analysis |
| **Curvature** | DEM | Ridge/valley detection via slope rate-of-change | Terrain classification |

### Terrain Characterization

| Metric | What It Measures | Application |
|--------|------------------|-------------|
| **TRI** (Terrain Ruggedness Index) | Local elevation variability | Habitat suitability, erosion |
| **TPI** (Topographic Position Index) | Position relative to neighbors | Ridge/valley classification |
| **Openness** | Convexity/concavity of terrain | Landform feature extraction |
| **Hillshade** | Simulated lighting from azimuth/altitude | Visual depth on 2D maps |
| **Contour/Isoline** | Lines of equal value | Gradient visualization |
| **Thalweg** | Lowest line in valley/channel | Stream network analysis |

### Hydrological Analysis

| Analysis | Description | Source Data |
|----------|-------------|-------------|
| **Flow Direction/Accumulation** | Water flow paths across DEM | SRTM, Copernicus DEM |
| **Watershed Delineation** | Contributing drainage area | DEM |
| **Drainage Network Extraction** | Stream channels from flow accumulation | DEM |
| **Flood Inundation** | Spatial extent at specified water levels | DEM + hydraulic models |

### Surface & Spatial Analysis

| Analysis | Description | Library |
|----------|-------------|---------|
| **KDE (Density)** | Spatial concentration heatmaps | `scipy.stats.gaussian_kde` |
| **Hot Spot (Getis-Ord Gi*)** | Statistically significant clusters | `PySAL.esda` |
| **Moran's I** | Spatial autocorrelation measure | `PySAL.esda` |
| **Interpolation** | Continuous surfaces from point data (IDW, Kriging, Spline) | `scipy`, `pykrige` |
| **Zonal Statistics** | Raster stats within polygon zones | `rasterstats` |
| **PCA** | Dimensionality reduction for multi-band rasters | `sklearn`, `rasterio` |
| **GWR** | Spatially varying regression coefficients | `mgwr` |
| **Point Pattern** | Clustered/dispersed/random distribution tests | `PySAL.esda` |

### Routing & Accessibility

| Analysis | Description | Tools |
|----------|-------------|-------|
| **Cost Distance** | Accumulated friction surface traversal | `rasterio`, `GRASS GIS` |
| **Least-Cost Path** | Optimal route minimizing cumulative cost | `rasterio`, `networkx` |
| **Isochrone** | Reachable area within time/distance | ORS, Valhalla, Mapbox |
| **Location-Allocation** | Optimal facility placement | `PySAL.spopt` |

### Signal & Propagation

| Analysis | Description | Use Case |
|----------|-------------|----------|
| **Radio/Cell Signal** | Coverage footprint over terrain | Telecom tower placement |
| **Noise Propagation** | Sound attenuation from source | Environmental impact assessment |
| **Solar Irradiance** | Energy reaching a surface | Solar panel siting |

### Multi-Criteria & Risk

| Analysis | Description | Tools |
|----------|-------------|-------|
| **Suitability (Weighted Overlay)** | Combined criteria scoring | `rasterio`, QGIS |
| **MCDA/AHP** | Structured multi-objective evaluation | `PySAL`, custom workflows |
| **Hazard Mapping** | Spatial hazard distribution | Terrain + historical data |
| **Exposure Analysis** | Assets at risk within hazard zones | Census + hazard footprints |
| **Vulnerability Assessment** | Composite risk scoring | `PySAL`, custom workflows |
| **Slope Stability** | Landslide susceptibility classification | DEM + soil + hydrology |

### Simplification & Tessellation

| Algorithm | Description | Use Case |
|-----------|-------------|----------|
| **Douglas-Peucker** | Line simplification preserving shape | Low-zoom tile rendering |
| **Chaikin's** | Corner-cutting curve smoothing | Animated path rendering |
| **Voronoi** | Proximity-based plane partitioning | Catchment areas, territories |
| **Delaunay** | Triangulation dual of Voronoi | TIN terrain meshes |

### Spatial Indexing

| Structure | Query Type | Used By |
|-----------|------------|---------|
| **R-tree** | Bounding-box range | PostGIS, SQLite, libspatialindex |
| **Quad-tree** | Recursive 2D subdivision | MongoDB, Elasticsearch |
| **K-D tree** | Nearest-neighbor | SciPy, Turf.js |
| **GeoHash** | String prefix proximity | Redis, DynamoDB |
| **H3** | Hexagonal hierarchical | PostGIS, MongoDB, Redis |
| **S2** | Spherical cells | Google, MongoDB |

---

## 2. Datasets & Data Extraction

### Raster Datasets

| Type | Description | Sources | Extraction |
|------|-------------|---------|------------|
| **DEM** | Elevation grid | SRTM (30m), ASTER GDEM, ALOS PALSAR (12.5m), Copernicus (30/90m) | `rasterio`, `GDAL` |
| **DSM** | Elevation + buildings/canopy | LiDAR, photogrammetry, TanDEM-X | `pdal`, `laspy` |
| **DTM** | Bare-earth elevation | LiDAR, SRTM | `pdal`, `LAStools` |
| **Optical Imagery** | Multi-spectral reflectance | Landsat 8/9 (30m), Sentinel-2 (10m), MODIS, Planet (3-5m) | `rasterio`, GEE, `pystac-client` |
| **SAR Imagery** | Cloud-penetrating radar | Sentinel-1, ALOS-2, TerraSAR-X | `sentinelsat`, `asf_search`, `snappy` |
| **Thermal** | Land surface temperature | Landsat TIRS, MODIS LST, ECOSTRESS | `rasterio`, GEE |
| **Climate** | Gridded weather variables | ERA5 (0.25°), WorldClim (1km), CHELSA, PRISM | `xarray`, `netCDF4`, `opendap` |
| **LULC** | Land cover classification | ESA WorldCover (10m), Dynamic World (10m), MODIS LUC | `rasterio`, GEE |
| **Soil** | Soil attributes (pH, texture) | SoilGrids (250m), HWSD (1km), OpenLandMap | `rasterio`, `soilDB` |
| **Vegetation Index** | NDVI/EVI/SAVI | Sentinel-2, Landsat, MODIS | `earthpy`, GEE |

### Vector Datasets

| Type | Description | Sources | Extraction |
|------|-------------|---------|------------|
| **Points** | Location markers with attributes | OSM nodes, USGS earthquakes, weather stations | `GeoPandas`, `osmnx`, `overpy` |
| **Lines** | Linear features (roads, rivers) | OSM ways, TIGER/Line, NHD | `osmnx`, `GeoPandas`, `ogr2ogr` |
| **Polygons** | Enclosed regions | OSM relations, census tracts, building footprints | `osmnx`, `GeoPandas`, `ogr2ogr` |
| **Admin Boundaries** | Political/admin polygons | GADM, Natural Earth, OSM, TIGER/Line | `GeoPandas`, `gadm` |
| **Building Footprints** | Building outlines | Microsoft, Google Open Buildings, OSM | `GeoPandas`, `ogr2ogr` |
| **Transportation** | Road/rail/transit networks | OSM, HERE, TomTom, GTFS | `osmnx`, GTFS parsers |
| **Hydrography** | River/stream/lake geometries | NHD, HydroSHEDS, OSM water | `osmnx`, `GeoPandas` |
| **Cadastral** | Land ownership parcels | Local gov systems, LINZ, LADM | `GeoPandas`, `ogr2ogr` |

### Point Cloud Datasets

| Type | Description | Sources | Extraction |
|------|-------------|---------|------------|
| **LiDAR** | Airborne/terrestrial laser scan | USGS 3DEP, NASA GEDI, OpenTopography | `pdal`, `laspy`, `entwine` |
| **Photogrammetric** | SfM/MVS from imagery | Drone stereo pairs, WorldView | `OpenDroneMap`, `COLMAP` |
| **Bathymetric** | Underwater sonar depth | NOAA, GEBCO | `xarray`, `netCDF4` |

### Network & Real-Time Datasets

| Type | Description | Sources | Extraction |
|------|-------------|---------|------------|
| **Road Graphs** | Edge-node transport networks | OSMnx, HERE, TomTom | `osmnx`, `networkx` |
| **GTFS Feeds** | Transit schedules/routes | Google GTFS, Transitland | `gtfs_functions`, `parade` |
| **Utility Networks** | Power/water/sewer grids | OSM, local utility open data | `osmnx`, `overpy` |
| **AIS/ADS-B Tracks** | Vessel/aircraft trajectories | MarineTraffic, FlightRadar24 | `pandas`, `streamz` |
| **Weather Feeds** | Real-time observations/forecasts | OpenWeatherMap, MET Norway, NOAA | `requests`, `xarray` |
| **Traffic Feeds** | Live flow/congestion | TomTom, HERE, Uber Movement | `requests`, `herepy` |
| **Seismic Feeds** | Earthquake/hazard alerts | USGS, EMSC, NOAA | `requests`, `obspy` |
| **IoT Sensors** | Distributed sensor readings | Air quality, water level, smart city | MQTT, `InfluxDB` |

### 3D & Volumetric Datasets

| Type | Description | Sources | Extraction |
|------|-------------|---------|------------|
| **CityGML/BIM** | Semantic 3D buildings (LOD0-3) | Cologne, Berlin CityGML; IFC files | `py3dtiles`, `ifcopenshell` |
| **Subsurface Geology** | Volumetric geological layers | Borehole data, geological surveys | `GemPy`, Leapfrog |
| **Ocean/Atmosphere** | 3D gridded water/air columns | HYCOM, ERA5, ROMS, WRF | `xarray`, `netCDF4` |

### Vector Formats Comparison

| Format | Type | Strengths | Weaknesses | Read With |
|--------|------|-----------|------------|-----------|
| **GeoJSON** | Vector | Human-readable, widely supported | Slow for large datasets | `GeoPandas`, `json` |
| **Shapefile** | Vector | Legacy standard, broad tool support | 2GB limit, encoding issues | `GeoPandas`, `ogr2ogr` |
| **GeoPackage** | Vector+Raster | Modern, SQLite-based, no size limit | Newer, less legacy support | `GeoPandas`, `fiona` |
| **GeoTIFF** | Raster | Industry standard for elevation/imagery | Large files | `rasterio`, `GDAL` |
| **MBTiles** | Tiles | Compact tile storage | Tile-only | `tippecanoe`, tile servers |
| **KML/KMZ** | Vector | Google Earth native | Limited spatial operations | `GeoPandas`, `ogr2ogr` |
| **COPC** | Point Cloud | Cloud-optimized LiDAR | Specialized format | `pdal`, `copc` readers |

---

## 3. Databases

### Spatial Database Comparison

| Feature | PostGIS | SpatiaLite | MySQL Spatial | Oracle Spatial | MongoDB | Elasticsearch |
|---------|---------|------------|---------------|----------------|---------|---------------|
| **Geometry types** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Geography (spherical)** | ✅ | Limited | ❌ | ✅ | ✅ | ✅ |
| **Spatial indexes** | GiST, BRIN, SP-GiST | R-tree | R-tree | R-tree | 2dsphere | geo_shape |
| **Raster support** | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Topology** | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |
| **3D geometry** | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |
| **Vector tiles (MVT)** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **Routing** | pgRouting | ❌ | ❌ | Network ext | ❌ | ❌ |
| **Point clouds** | pgpointcloud | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Geocoding** | Tiger Geocoder | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Embedded** | ❌ | ✅ (SQLite) | ❌ | ❌ | ❌ | ❌ |
| **JSON export** | `ST_AsGeoJSON` | `ST_AsGeoJSON` | `ST_AsGeoJSON` | `SDO_UTIL.TO_GEOJSON` | Native | Native |

### Key Spatial SQL Functions

| Category | PostGIS | MySQL | SpatiaLite |
|----------|---------|-------|------------|
| **Measurement** | `ST_Area`, `ST_Length`, `ST_Distance`, `ST_Azimuth` | `ST_Area`, `ST_Length`, `ST_Distance` | `ST_Area`, `ST_Length`, `ST_Distance` |
| **Relationships** | `ST_Contains`, `ST_Intersects`, `ST_Within`, `ST_Crosses` | `ST_Contains`, `ST_Intersects`, `ST_Within` | `ST_Contains`, `ST_Intersects`, `ST_Within` |
| **Set Operations** | `ST_Union`, `ST_Intersection`, `ST_Difference` | `ST_Union`, `ST_Intersection`, `ST_Difference` | `ST_Union`, `ST_Intersection`, `ST_Difference` |
| **Overlay** | `ST_Buffer`, `ST_Simplify`, `ST_MakeValid` | `ST_Buffer`, `ST_Envelope`, `ST_MakeValid` | `ST_Buffer`, `ST_Simplify`, `ST_MakeValid` |
| **Centroids** | `ST_Centroid`, `ST_ClosestPoint`, `ST_PointOnSurface` | `ST_Centroid` | `ST_Centroid` |
| **Conversion** | `ST_GeomFromGeoJSON`, `ST_AsGeoJSON`, `ST_AsMVT` | `ST_GeomFromGeoJSON`, `ST_AsGeoJSON` | `ST_GeomFromGeoJSON`, `ST_AsGeoJSON` |
| **Clustering** | `ST_ClusterDBSCAN`, `ST_ClusterKMeans` | ❌ | ❌ |
| **TIN/Topology** | `ST_DelaunayTriangles`, `ST_VoronoiDiagrams` | ❌ | `ST_Triangulate2DZ`, `ST_DelaunayTriangles` |
| **Tiles** | `ST_AsMVT`, `ST_TileEnvelope` | ❌ | ❌ |

### PostgreSQL Extensions

| Extension | Purpose |
|-----------|---------|
| [PostGIS](https://postgis.net/) | Spatial SQL engine (geometry, geography, raster, topology, MVT) |
| [pgRouting](https://pgrouting.org/) | Network routing (Dijkstra, A*, isochrones, TSP, VRP) |
| [H3-Pg](https://github.com/bytesandbrains/h3-pg) | Uber H3 hexagonal index in SQL |
| [pgpointcloud](https://github.com/pgpointcloud/pointcloud) | LiDAR point cloud storage |
| [pg_tileserv](https://github.com/CrunchyData/pg_tileserv) | Vector tile server from PostGIS |
| [pg_featureserv](https://github.com/CrunchyData/pg_featureserv) | OGC API Features from PostGIS |
| [Citus](https://www.citusdata.com/) | Distributed PostgreSQL for large spatial datasets |
| [TimescaleDB](https://www.timescale.com/) | Time-series + PostGIS for spatiotemporal data |
| [PostGIS SFCGAL](https://postgis.net/documentation/reference/#sfcgal) | 3D geometry operations |
| [pg_analytics](https://github.com/paradedb/paradedb) | Analytics functions for PostGIS |

### SQLite/SpatiaLite Extensions

| Extension | Purpose |
|-----------|---------|
| [SpatiaLite](https://www.gaia-gis.it/gaia-sins/) | Full spatial SQL for SQLite (geometry, topology, TIN) |
| [DuckDB Spatial](https://duckdb.org/docs/extensions/spatial.html) | Embedded spatial analytics (GeoJSON, Shapefile I/O) |
| [sqlite-geopoly](https://www.sqlite.org/geopoly.html) | Polygon overlap queries |
| [ogr_fdw](https://github.com/pramsey/pgsql-ogr-fdw) | Foreign data wrapper for OGR sources |

### NoSQL Spatial Support

| Database | Index Type | Key Operations |
|----------|------------|----------------|
| [MongoDB](https://www.mongodb.com/docs/manual/geospatial-queries/) | 2dsphere | `$geoWithin`, `$geoIntersects`, `$near`, `geoNear` |
| [Redis](https://redis.io/docs/data-types/geospatial/) | GEO commands | `GEOADD`, `GEODIST`, `GEOSEARCH` |
| [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo.html) | geo_shape | `geo_bounding_box`, `geo_distance`, `geo_polygon` |

### Vector Tile Servers

| Tool | Backend | Language |
|------|---------|----------|
| [Martin](https://github.com/maplibre/martin) | PostGIS / MBTiles / PMTiles | Rust |
| [pg_tileserv](https://github.com/CrunchyData/pg_tileserv) | PostGIS | Go |
| [TileServer GL](https://github.com/mapbox/tileserver-gl) | MBTiles / PMTiles | Node.js |
| [Tegola](https://tegola.io/) | PostGIS | Go |
| [GeoServer](https://geoserver.org/) | Various OGC sources | Java |
| [MapProxy](https://mapproxy.org/) | WMS/WMTS sources | Python |

---

## 4. Libraries by Language

### Core C/C++ Libraries (Language-Agnostic)

| Library | Purpose |
|---------|---------|
| [GDAL](https://gdal.org/) | Raster + vector data abstraction (powers most GIS tools) |
| [GEOS](https://libgeos.org/) | Geometry engine (JTS C++ port) |
| [PROJ](https://proj.org/) | Coordinate reference system transformations |
| [libspatialindex](https://github.com/libspatialindex/libspatialindex) | R-tree spatial indexing |
| [libosmium](https://github.com/osmcode/libosmium) | Fast OSM data processing |
| [Mapnik](https://mapnik.org/) | Server-side 2D map rendering |
| [Boost.Geometry](https://www.boost.org/doc/libs/1_85_0/libs/geometry/doc/index.html) | Geometric algorithms |

### JavaScript / TypeScript

| Category | Library | Description |
|----------|---------|-------------|
| **Map Rendering** | [Leaflet](https://leafletjs.com/) | Mobile-friendly raster tile maps |
| | [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/) | WebGL vector maps (open-source) |
| | [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/) | WebGL vector maps (proprietary) |
| | [OpenLayers](https://openlayers.org/) | Raster + vector maps |
| | [ESRI ArcGIS JS API](https://developers.arcgis.com/javascript/) | Enterprise GIS SDK |
| | [Google Maps JS API](https://developers.google.com/maps/documentation/javascript) | Google Maps embedding |
| **Analysis** | [Turf.js](https://turfjs.org/) | Client-side geospatial analysis |
| | [JSTS](https://github.com/bjornharrtell/jsts) | Java topology suite port |

### Python

| Category | Library | Description |
|----------|---------|-------------|
| **Core Analysis** | [GeoPandas](https://geopandas.org/) | Spatial dataframes (Pandas + geometry) |
| | [Shapely](https://shapely.readthedocs.io/) | Geometric operations |
| | [Fiona](https://fiona.readthedocs.io/) | Vector data I/O |
| | [Rasterio](https://rasterio.readthedocs.io/) | GDAL-based raster I/O |
| | [pyproj](https://pyproj4.github.io/pyproj/) | Coordinate transformations |
| | [Rtree](https://rtree.readthedocs.io/) | R-tree spatial indexing |
| **Advanced** | [PySAL](https://pysal.org/) | Spatial statistics (hotspots, autocorrelation, regression) |
| | [mgwr](https://pysal.org/mgwr/) | Geographically weighted regression |
| | [tobler](https://pysal.org/tobler/) | Areal interpolation |
| **Visualization** | [Folium](https://python-visualization.github.io/folium/) | Leaflet maps (Jupyter) |
| | [Kepler.gl](https://kepler.gl/) | Geospatial data exploration |
| | [IPyleaflet](https://github.com/jupyter-widgets/ipyleaflet) | Leaflet for Jupyter widgets |
| **Routing** | [OSMnx](https://github.com/gboeing/osmnx) | Download + model street networks |
| | [networkx](https://networkx.org/) | Graph algorithms |
| | [Pandana](https://github.com/UDST/pandana) | Urban network analysis |
| **Geocoding** | [geopy](https://geopy.readthedocs.io/) | Multi-geocoder client |
| **OSM** | [PyOsmium](http://osmcode.org/pyosmium/) | PBF/XML processing |
| | [overpy](https://pypi.org/project/overpy/) | Overpass API client |
| | [osm2geojson](https://github.com/aspectumapp/osm2geojson) | OSM to GeoJSON |
| **Tiles** | [Martin](https://github.com/maplibre/martin) | PostGIS vector tile server |
| | [Kartograph](https://github.com/kartograph/kartograph.py) | Map rendering framework |

### Java / Kotlin

| Category | Library | Description |
|----------|---------|-------------|
| **GIS Core** | [GeoTools](https://geotools.org/) | Java GIS toolkit (OGC standards) |
| | [JTS](https://locationtech.github.io/jts/) | Java Topology Suite |
| | [GeoAPI](https://geoapi.org/) | OGC API interfaces |
| **Map Rendering** | [osmdroid](https://github.com/osmdroid/osmdroid) | Android map view (OSM-based) |
| | [MapLibre Native](https://github.com/maplibre/maplibre-native) | Cross-platform vector/raster rendering |
| | [Tangram ES](https://github.com/tangrams/tangram-es) | OpenGL ES 2D/3D map renderer |
| | [mapsforge](https://github.com/mapsforge/mapsforge) | Android offline rendering |
| **Routing** | [GraphHopper](https://www.graphhopper.com/) | Routing engine (car, bike, foot) |
| | [OSRM Java](https://github.com/Project-OSRM/osrm-backend) | OSRM bindings |
| | [OpenTripPlanner](https://www.opentripplanner.org/) | Multimodal transit routing |
| **OSM** | [osm4j](https://github.com/topobyte/osm4j) | PBF/XML reader |
| | [Osmosis](https://github.com/openstreetmap/osmosis) | OSM data processing pipeline |
| | [GeoDesk](https://geodesk.com/) | Fast OSM data engine |
| **Geocoding** | [Gisgraphy](http://www.gisgraphy.com/) | Geocoding/reverse geocoding server |

### Swift (iOS/macOS)

| Category | Library | Description |
|----------|---------|-------------|
| **Maps** | [MapLibre Native iOS](https://github.com/maplibre/maplibre-native) | Open-source vector maps |
| | [Apple MapKit](https://developer.apple.com/mapkit/) | Native Apple Maps |
| **Navigation** | [Mapbox Navigation iOS](https://github.com/mapbox/mapbox-navigation-ios) | Turn-by-turn navigation |
| | [MapLibre Navigation iOS](https://github.com/maplibre/maplibre-navigation-android) | Turn-by-turn navigation |
| **Geocoding** | [Mapbox Geocoder for Swift](https://github.com/mapbox/mapboxgeocoder.swift) | Geocoding client |

### C# / .NET

| Category | Library | Description |
|----------|---------|-------------|
| **Maps** | [Mapsui](https://github.com/Mapsui/Mapsui) | Cross-platform map control |
| | [BruTile](https://github.com/BruTile/BruTile) | Tile source abstraction |
| **Analysis** | [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) | JTS port (geometry operations) |
| | [ProjNET](https://github.com/NetTopologySuite/ProjNet) | Coordinate transformations |
| | [SharpMap](https://github.com/SharpMap/SharpMap) | GIS mapping engine |
| | [DotSpatial](https://github.com/DotSpatial/DotSpatial) | GIS library |
| **Routing** | [Itinero](https://github.com/itinero/routing) | Open-source routing engine |
| **OSM** | [OsmSharp](https://github.com/xivk/osmsharp) | OSM data + offline rendering |

### C/C++

| Category | Library | Description |
|----------|---------|-------------|
| **Map Rendering** | [Mapnik](https://mapnik.org/) | Industry-standard 2D renderer |
| | [MapLibre Native](https://github.com/maplibre/maplibre-native) | Cross-platform vector/raster |
| | [Tangram ES](https://github.com/tangrams/tangram-es) | OpenGL ES renderer |
| **Routing** | [OSRM](https://github.com/Project-OSRM/osrm-backend) | Open-source routing machine |
| | [Valhalla](https://github.com/valhalla/valhalla) | Open-source routing engine |
| | [Routino](https://github.com/routino/routino) | Flexible OSM router |
| **Tiles** | [libprotobuf](https://github.com/protocolbuffers/protobuf) | Protocol Buffers (vector tiles) |
| | [protozero](https://github.com/mapbox/protozero) | Minimal protobuf decoder |

### Rust

| Category | Library | Description |
|----------|---------|-------------|
| **Core** | [geo](https://github.com/georust/geo) | Geospatial primitives + algorithms |
| | [geos](https://github.com/georust/geos) | GEOS bindings |
| | [proj](https://github.com/georust/proj) | PROJ bindings |
| | [geojson](https://github.com/georust/geojson) | GeoJSON parsing |
| | [GDAL bindings](https://github.com/georust/gdal) | GDAL bindings |
| **Indexing** | [rstar](https://github.com/georust/rstar) | R-tree spatial indexing |
| | [kdtree](https://github.com/mneumark/kdtree) | K-D tree |
| **Routing** | [Valhalla](https://github.com/valhalla/valhalla) | C++ routing (Rust bindings) |
| **Tiles** | [Martin](https://github.com/maplibre/martin) | PostGIS/MBTiles vector tile server |

### Go

| Category | Library | Description |
|----------|---------|-------------|
| **Core** | [geom](https://github.com/paulmach/geom) | Geometric types + operations |
| | [s2](https://github.com/golang/geo) | S2 geometry (Google) |
| | [H3-go](https://github.com/uber/h3-go) | H3 hexagonal index |
| **OSM** | [gOSMonaut](https://github.com/MorbZ/gosmonaut) | PBF parser |
| | [gosmparse](https://github.com/thomersch/gosmparse) | High-speed PBF parser |
| **Routing** | [go-osrm](https://github.com/paulmach/go-osrm) | OSRM API client |

### R

| Category | Library | Description |
|----------|---------|-------------|
| **Core** | [sf](https://r-spatial.github.io/sf/) | Simple Features (vector data) |
| | [terra](https://rspatial.org/terra/) | Spatial raster data |
| | [stars](https://r-spatial.github.io/stars/) | Spatiotemporal arrays |
| | [sp](https://cran.r-project.org/package=sp) | Spatial classes (legacy) |
| **Visualization** | [leaflet](https://rstudio.github.io/leaflet/) | Leaflet for R |
| | [tmap](https://r-tmap.github.io/tmap/) | Thematic map visualization |
| **OSM** | [osmdata](https://github.com/ropensci/osmdata) | OSM data via Overpass API |
| **Routing** | [OpenRouteService](https://github.com/GIScience/openrouteservice-r) | ORS routing client |

### Other Languages

| Language | Library | Description |
|----------|---------|-------------|
| **Scala** | [geotrellis](https://geotrellis.io/) | Geospatial data processing (raster + vector) |
| **Ruby** | [rgeo](https://github.com/rgeo/rgeo) | Geospatial library (GEOS bindings) |
| **PHP** | [GeoPHP](https://github.com/phayes/geoPHP) | Geometry library (WKT, GeoJSON) |
| **Julia** | [ArchGDAL.jl](https://github.com/JuliaGeo/ArchGDAL.jl) | GDAL bindings |
| **Haskell** | [haskell-gis](https://hackage.haskell.org/package/gis) | GIS data structures |
| **Lua** | [TileMan](https://github.com/osmfj/tileman/) | OSM tile serving framework |

---

## 5. APIs & Services

### Routing Engines Comparison

| Engine | License | Language | Features |
|--------|---------|----------|----------|
| [OSRM](http://project-osrm.org/) | BSD | C++ | Fastest, car/bike/foot, MLD algorithm |
| [Valhalla](https://valhalla.github.io/) | MIT | C++ | Costing modes, isochrones, elevation, time-dependent |
| [GraphHopper](https://www.graphhopper.com/) | Apache 2.0 | Java | Flexible profiles, contraction hierarchies |
| [OpenRouteService](https://openrouteservice.org/) | GPL | Java | Multimodal, wheelchair accessible, isochrones |
| [OpenTripPlanner](https://www.opentripplanner.org/) | LGPL | Java | Multimodal transit + car + bike + walk |

### Geocoding Services

| Service | Type | Coverage |
|---------|------|----------|
| [Nominatim](https://nominatim.org/) | Self-hostable | Global (OSM-based) |
| [Pelias](https://github.com/pelias/pelias) | Self-hostable | Global (multiple sources) |
| [Photon](https://photon.komoot.io/) | Self-hostable | Global (OSM-based) |
| [Google Geocoding](https://developers.google.com/maps/documentation/geocoding) | Commercial | Global |
| [HERE Geocoding](https://developer.here.com/geocoding-api) | Commercial | Global |

### Tile Services

| Service | Type | Format |
|---------|------|--------|
| [OpenStreetMap Tiles](https://www.openstreetmap.org/wiki/Tile_servers) | Free | Raster |
| [Protomaps](https://protomaps.com/) | Open-source | Vector (PMTiles) |
| [MapTiler](https://www.maptiler.com/) | Commercial | Vector + Raster |
| [Stadia Maps](https://stadiamaps.com/) | Commercial | Vector |
| [Mapbox](https://www.mapbox.com/) | Commercial | Vector + Raster |

### Weather & Traffic APIs

| Service | Type |
|---------|------|
| [OpenWeatherMap](https://openweathermap.org/) | Weather data + tile overlays |
| [Tomorrow.io](https://www.tomorrow.io/) | Weather API |
| [MET Norway](https://api.met.no/) | Free weather API |
| [TomTom Traffic](https://developer.tomtom.com/traffic-api) | Traffic flow + incidents |
| [HERE Traffic](https://developer.here.com/traffic-api) | Traffic flow + incidents |
| [Azure Maps Traffic](https://docs.microsoft.com/rest/api/maps/traffic) | Traffic flow + incidents |

---

## 6. Frameworks & Integrations

### Django GIS (GeoDjango)

| Feature | Description |
|---------|-------------|
| **Field Types** | `PointField`, `LineStringField`, `PolygonField`, `MultiPolygonField`, `GeometryCollectionField` |
| **Query Lookups** | `contains`, `within`, `intersects`, `touches`, `overlaps`, `crosses`, `disjoint` |
| **Distance Queries** | `distance_lte`, `distance_gte`, `dwithin` |
| **Spatial Functions** | `Area`, `Distance`, `Length`, `Buffer`, `Simplify`, `Intersection`, `Difference`, `Union` |
| **Backends** | PostGIS, SpatiaLite, MySQL, Oracle |
| **Feed Formats** | GeoRSS, GeoJSON, KML, KMZ |

### Data Pipeline Tools

| Tool | Description |
|------|-------------|
| [Tippecanoe](https://github.com/felt/tippecanoe) | GeoJSON to MBTiles with clustering/simplification |
| [ogr2ogr](https://gdal.org/programs/ogr2ogr.html) | Format conversion (GeoJSON, Shapefile, KML, PostGIS) |
| [PMTiles](https://protomaps.com/) | Single-file cloud-native tile archives |
