# Awesome Geo Resources

> **Complementary reference material for the geospatial ecosystem — algorithms, libraries, databases, and service APIs.**

This document contains non-example reference material extracted from [Awesome Maps Examples](./README.md). For interactive map examples, see the main README.

---


### Distance & Earth Models
* **Haversine Formula**: The standard mathematical formula to calculate the *great-circle distance* between two points on a sphere given their longitudes and latitudes. It is fast but assumes a perfectly spherical Earth.
* **Vincenty's Formulae**: A highly accurate iterative method to calculate distance between two points on the surface of a spheroid (specifically using the WGS-84 ellipsoid model). It is more computationally expensive than Haversine but accurate to within half a millimeter.

### Point & Polygon Analysis
* **Ray-Casting Algorithm**: The foundational algorithm used to determine if a point is inside a polygon (`Point-in-Polygon`). It works by shooting an imaginary ray from the point in a single direction and counting how many times it intersects the polygon's edges. An odd number means the point is inside; an even number means it is outside.
* **Convex Hull**: An algorithm that finds the smallest convex polygon enclosing a given set of points (imagine stretching a rubber band around a set of pegs). Commonly used to define the bounding area of a clustered dataset.

### 2D Perspective & Visibility Analysis
* **Viewshed Analysis**: Determines the regions on a 2D map (or 2.5D elevation model) that are visually unobstructed from a specific vantage point (or set of points). Commonly used in urban planning, telecommunications (e.g., cell tower placement), and military logistics.
  * **Dataset & Extraction**: Relies heavily on **Digital Surface Models (DSM)** or **Digital Terrain Models (DTM)**. Free global datasets include SRTM (Shuttle Radar Topography Mission) and Copernicus DEM, accessible via Google Earth Engine or AWS Open Data.
* **Line of Sight (LoS)**: A computational calculation to determine if there is a direct, unobstructed visual path between an observer and a target point across a terrain or a 2D plane with extruded obstacles (like buildings).
  * **Dataset & Extraction**: Requires high-resolution **LiDAR point clouds** or 3D city models. OpenStreetMap (OSM) can provide 2.5D building footprints via the `building:levels` or `height` tags, easily extractable using the Overpass API.
* **Topographic Profiling**: Generating a 2D cross-section graph of elevation or surface attribute changes along a specified linear path, allowing users to understand a terrain's relief from a side-on perspective.
  * **Dataset & Extraction**: Uses **Digital Elevation Models (DEM)**. The Mapzen/Nextzen terrain tiles (Terrarium formats) or USGS 3DEP program provide excellent APIs for extracting elevation along a path.
* **Aspect & Slope Analysis**: Calculating the steepness (slope) and the compass direction that a terrain surface faces (aspect) from a 2.5D Digital Elevation Model (DEM). This is typically rendered back onto a 2D map as a color ramp or hillshade to give the illusion of depth.
  * **Dataset & Extraction**: Uses **DEMs/DTMs**. You can extract and process this data using GDAL (`gdaldem slope` and `gdaldem aspect` commands) directly from GeoTIFFs downloaded from EarthExplorer or OpenTopography.
* **Shadow & Solar Radiation Analysis**: Simulating the shadows cast by topography and 2.5D building footprints onto a 2D map plane over a specific time period. Crucial for solar panel placement, agriculture, and urban microclimate design.
  * **Dataset & Extraction**: Needs precise **DSMs** that include tree canopies and buildings. Overture Maps Foundation and Microsoft's Global Building Footprints provide extensive datasets that can be cross-referenced with solar ephemeris APIs (like SunCalc).
* **Skyline / Prominence Analysis**: Determining the visual prominence of features (such as mountain peaks or skyscrapers) as they appear against the sky from a 2D observer's ground-level perspective.
  * **Dataset & Extraction**: Requires topographical prominence databases and **LiDAR**. Peakbagger/OpenStreetMap peak nodes and localized open government LiDAR datasets (e.g., from national geographic institutes) provide the raw data needed for these visibility calculations.
* **Cost Distance & Least-Cost Path Analysis**: Calculating the accumulated cost (or friction) of traversing a 2D surface from a source point, factoring in variables like slope, land cover, and barriers. The least-cost path identifies the optimal route minimizing cumulative cost between two points.
* **Density Analysis**: Estimating the spatial concentration of features (points, lines, polygons) across a 2D surface using techniques like Kernel Density Estimation (KDE), creating heatmaps that reveal hot spots and cold spots.
* **Hot Spot Analysis (Getis-Ord Gi*)**: Identifying statistically significant spatial clusters of high values (hot spots) or low values (cold spots) within a dataset, accounting for spatial autocorrelation.
* **Spatial Autocorrelation (Moran's I)**: Measuring the degree to which nearby locations exhibit similar values, quantifying spatial clustering or dispersion patterns in a 2D plane.
* **Interpolation**: Creating continuous surfaces from discrete point measurements using methods like Inverse Distance Weighting (IDW), Kriging, or Spline interpolation.
* **Zonal Statistics**: Computing summary statistics (mean, min, max, sum, std dev) of raster values within defined zones (polygons), essential for aggregating terrain or climate data by administrative boundaries.
* **Isochrone & Service Area Analysis**: Mapping areas reachable from a point within specified time or distance thresholds, used for accessibility studies, emergency response planning, and retail catchment analysis.
* **Watershed Delineation**: Identifying the contributing drainage area for a specific outlet point on a DEM, critical for flood risk assessment and hydrological modeling.
* **Curvature Analysis**: Calculating the rate of slope change across a terrain surface to identify convex (ridges) and concave (valleys) features, useful for terrain characterization and erosion prediction.
* **Hillshade & Relief Shading**: Rendering 3D terrain relief on a 2D map by simulating lighting effects from a specified azimuth and altitude, creating visual depth perception.
* **Contour & Isoline Generation**: Extracting lines of equal value (elevation, temperature, pressure) from raster surfaces to visualize gradients and terrain form.
* **Principal Component Analysis (PCA)**: Reducing dimensionality of multi-band raster data (e.g., satellite imagery) to identify dominant spatial patterns and noise components.

### Terrain Characterization & Geomorphometry
* **Terrain Ruggedness Index (TRI)**: Quantifying the local variability of elevation values within a neighborhood, measuring terrain roughness and complexity. Useful for habitat suitability, erosion modeling, and terrain classification.
* **Topographic Position Index (TPI)**: Classifying a terrain point's position relative to its surroundings — identifying ridges, valleys, and slope breaks by comparing elevation to the mean elevation of a defined neighborhood.
* **Sky-View Factor (SVF)**: Calculating the proportion of sky visible from a ground-level point, accounting for obstruction by terrain and buildings. Critical for urban canyon analysis, solar access studies, and microclimate modeling.
* **Openness (Positive & Negative)**: Measuring the degree of convexity (positive openness — how much terrain rises around a point) or concavity (negative openness — how enclosed a point is), useful for landform feature extraction.
* **Terrain Classification (Landform Elements)**: Automatically classifying terrain into geomorphometric categories (peaks, ridges, passes, channels, valleys, foot slopes, planar surfaces) using slope, curvature, and TPI thresholds.
* **Talweg / Thalweg Line Extraction**: Identifying the line of lowest elevation along a valley floor or channel, representing the deepest part of a watercourse or depression.

### Hydrological Surface Analysis
* **Flow Direction & Flow Accumulation**: Computing the direction of steepest descent for each cell in a DEM and accumulating upstream cell counts to model water flow paths and watershed structure.
* **Drainage Network Extraction**: Deriving stream channel networks from flow accumulation grids using threshold-based or curvature-based methods, fundamental for hydrological modeling and flood analysis.
* **Catchment / Drainage Area Delineation**: Automatically delineating the total contributing area upstream of any point on a terrain surface, defining hydrological boundaries for water resource management.
* **Flood Inundation Mapping**: Simulating the spatial extent of flooding at specified water levels using DEM-based hydraulic modeling, identifying areas at risk of submersion.

### Signal & Propagation Modeling
* **Signal Propagation Analysis**: Modeling the 2D footprint of radio, TV, or cellular signal coverage over terrain, accounting for diffraction, reflection, and attenuation caused by topography and obstacles.
* **Noise Propagation Modeling**: Simulating sound level attenuation from a noise source (highway, airport, industrial) across a terrain surface, incorporating ground absorption and obstacle shielding for environmental impact assessment.
* **Solar Irradiance Mapping**: Estimating the spatial distribution of solar energy reaching a 2D surface, combining slope, aspect, shadow, and atmospheric factors to identify optimal solar panel installation zones.

### Multi-Criteria & Suitability Analysis
* **Suitability Analysis (Weighted Overlay)**: Combining multiple spatial criteria (slope, land use, proximity, soil type) into a single suitability map using weighted raster algebra, essential for site selection and land-use planning.
* **Multi-Criteria Decision Analysis (MCDA)**: A structured framework for evaluating spatial alternatives against conflicting objectives (e.g., environmental protection vs. development), often using Analytic Hierarchy Process (AHP) with GIS overlay.
* **Location-Allocation Analysis**: Optimally placing facilities (hospitals, fire stations, warehouses) to serve demand points while minimizing travel distance or maximizing coverage within a network or 2D space.

### Spatial Change Detection
* **Multi-Temporal Change Detection**: Comparing geospatial datasets from different time periods to identify areas of change (urban expansion, deforestation, coastal erosion), using pixel-based, object-based, or vector-based differencing methods.
* **Land Use / Land Cover (LULC) Classification**: Categorizing terrain into land cover types (forest, water, urban, agriculture) from satellite or aerial imagery using supervised or unsupervised classification algorithms.
* **Urban Growth Modeling**: Simulating and predicting future urban expansion patterns using cellular automata, agent-based models, or regression-based approaches driven by historical LULC change data.

### Spatial Statistics & Regression
* **Geographically Weighted Regression (GWR)**: Extending ordinary regression to model spatially varying relationships between variables, allowing regression coefficients to change across the study area rather than assuming stationarity.
* **Spatial Outlier Detection**: Identifying locations with significantly different attribute values compared to their spatial neighbors, useful for quality control, fraud detection, and anomaly identification in geospatial datasets.
* **Point Pattern Analysis**: Analyzing the spatial distribution of point features using Nearest Neighbor Index, Quadrat Analysis, or Ripley's K-function to determine if patterns are clustered, dispersed, or random.

### Risk & Hazard Assessment
* **Hazard Mapping**: Visualizing the spatial distribution of natural or anthropogenic hazards (flood zones, landslide susceptibility, wildfire risk, seismic shaking intensity) using terrain, geology, and historical event data.
* **Exposure Analysis**: Quantifying the population, infrastructure, or assets located within hazard zones, combining hazard footprints with census, building footprint, or asset inventory data.
* **Vulnerability Assessment**: Integrating hazard likelihood, exposure, and resilience indicators into a composite risk score for each spatial unit, supporting disaster preparedness and climate adaptation planning.
* **Slope Stability Analysis**: Evaluating landslide susceptibility by combining slope angle, soil properties, vegetation cover, and hydrological conditions to classify terrain into stability risk categories.

### Geospatial Dataset Types & Data Extraction

#### Raster Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **Digital Elevation Model (DEM)** | Grid of elevation values representing terrain surface | SRTM (30m), ASTER GDEM (30m), ALOS PALSAR (12.5m), LiDAR-derived DEM (1m+), Copernicus DEM (30m/90m) | `rasterio`, `GDAL`, `earthpy`, `GDAL_translate` |
| **Satellite Imagery (Optical)** | Multi-spectral aerial/space imagery capturing land surface reflectance | Landsat 8/9 (30m, 11 bands), Sentinel-2 (10m, 13 bands), MODIS (250m–1km), Planet Labs (3–5m), VIIRS | `rasterio`, `Google Earth Engine`, `SentinelHub`, `pystac-client`, `landsatxplore` |
| **Satellite Imagery (Radar/SAR)** | Active microwave imagery penetrating clouds, useful for surface deformation and flood mapping | Sentinel-1 (C-band SAR), ALOS-2 (L-band), RADARSAT, TerraSAR-X | `sentinelsat`, `asf_search`, `snappy` (SNAP toolbox), `sarsen` |
| **Thermal Imagery** | Captures land surface temperature from satellite thermal bands | Landsat 8/9 TIRS (Band 10/11), MODIS LST (1km), ECOSTRESS (70m) | `rasterio`, `earthpy`, `Google Earth Engine` |
| **Climate & Weather Rasters** | Gridded climate variables (precipitation, temperature, wind) | ERA5 (0.25°), WorldClim (1km), CHELSA (1km), NCEP/NCAR, PRISM (800m) | `xarray`, `netCDF4`, `opendap`, `climate-indices` |
| **Land Use / Land Cover (LULC)** | Classified raster maps of surface cover types | ESA WorldCover (10m), MODIS LUC (500m), CLCD (30m), Dynamic World (10m), GlobeLand30 | `rasterio`, `Google Earth Engine`, `landcover` |
| **Soil Property Rasters** | Gridded soil attributes (texture, pH, organic carbon) | SoilGrids (250m), HWSD (1km), OpenLandMap, ISRIC SoilGrids 2.0 | `rasterio`, `soilDB`, `gsodr` |
| **Vegetation Index Rasters** | Derived spectral indices quantifying vegetation health/density | NDVI, EVI, SAVI from Sentinel-2/Landsat, MODIS NDVI (16-day composites) | `earthpy`, `rasterio`, `Google Earth Engine` |
| **Digital Surface Model (DSM)** | Elevation including buildings and vegetation canopy | LiDAR DSM, photogrammetry DSM, TanDEM-X (12m) | `rasterio`, `pdal`, `LASlib` |
| **Digital Terrain Model (DTM)** | Bare-earth elevation (buildings/vegetation stripped) | LiDAR DTM, SRTM, Copernicus DEM | `rasterio`, `pdal`, `LAStools` |

#### Vector Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **Points** | Individual location markers with attributes (POIs, sensors, incidents) | OpenStreetMap nodes, USGS earthquake epicenters, weather stations, event locations | `GeoPandas`, `osmnx`, `overpy`, `requests` (API), `ogr2ogr` |
| **Lines** | Polylines representing linear features (roads, rivers, pipelines) | OpenStreetMap ways, TIGER/Line road networks, NHD hydrography, utility networks | `osmnx`, `GeoPandas`, `shapely`, `ogr2ogr`, `pbf` parsing |
| **Polygons** | Enclosed areas representing regions (parcels, districts, buildings) | OpenStreetMap relations, census tracts, cadastral parcels, building footprints (Microsoft/Google) | `osmnx`, `GeoPandas`, `ogr2ogr`, `tippecanoe` |
| **Multi-Geometries** | Collections of points, lines, or polygons treated as a single feature | Admin boundaries (MultiPolygon), river basins, utility service areas | `GeoPandas`, `shapely`, `ogr2ogr` |
| **Administrative Boundaries** | Political/administrative polygon datasets at various scales | GADM, Natural Earth, OSM admin boundaries, census TIGER/Line, Eurostat NUTS | `GeoPandas`, `gadm`, `osmnx`, `census` (R/Python) |
| **Building Footprints** | Polygon outlines of building structures | Microsoft Building Footprints, Google Open Buildings, OSM buildings, Ordnance Survey | `GeoPandas`, `ogr2ogr`, `tippecanoe` |
| **Transportation Networks** | Road, rail, transit, and cycling network geometries | OpenStreetMap, HERE, TomTom, GTFS (transit routes/shapes), TIGER/Line | `osmnx`, `GTFS parsers`, `overpy`, `parade` |
| **Hydrography Networks** | River, stream, lake, and watershed geometries | NHD (US), HydroSHEDS, OSM water features, Global River Thickness Database | `osmnx`, `GeoPandas`, `hydrosheds`, `nhdpy` |
| **Cadastral / Land Parcel Data** | Legal land ownership boundaries | Local government cadastral systems, OSM land parcels, LINZ (NZ), LADM standard | `GeoPandas`, `ogr2ogr`, `LADM` APIs |

#### Point Cloud Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **LiDAR Point Clouds** | 3D point measurements from airborne/terrestrial laser scanning | USGS 3DEP LiDAR, NASA GEDI (forest canopy), OpenTopography, local government LiDAR portals | `pdal`, `LASlib`, `laspy`, `copc` readers, `entwine` |
| **Photogrammetric Point Clouds** | 3D points derived from overlapping aerial/satellite stereo imagery | SfM/MVS from drone imagery, satellite stereo pairs (WorldView, Pléiades) | `OpenDroneMap`, `MicMac`, `Agisoft Metashape`, `COLMAP` |
| ** bathymetric Point Clouds** | Underwater depth measurements (sonar-derived) | NOAA nautical charts, GEBCO bathymetry, IHO Data Centre | `GeoPandas`, `netCDF4`, `xarray` |
| **Mobile Mapping Point Clouds** | Terrestrial laser scans from vehicle-mounted sensors | Google Street View LiDAR, HERE HD Live Map, TomTom survey data | `pdal`, `laspy`, proprietary SDKs |

#### Tabular / CSV Datasets with Coordinates
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **CSV with lat/lon columns** | Flat files containing longitude/latitude attributes | FEMA disaster declarations, EPA air quality monitors, USDA crop yields, crime incidents | `GeoPandas.read_file()`, `pd.read_csv()` + `points_from_xy()` |
| **GeoJSON** | JSON-based format for geographic data features | OSM exports, GitHub geo-data repos, government open data portals | `GeoPandas.read_file()`, `json` module, `shapely` |
| **KML / KMZ** | Google Earth markup format | Google Earth exports, government KML feeds, FEMA flood maps | `GeoPandas`, `ogr2ogr`, `fastkml`, `simplekml` |
| **Shapefile (.shp)** | Legacy binary vector format (still widely distributed) | Census data, TIGER/Line, historical GIS datasets, government portals | `GeoPandas.read_file()`, `fiona`, `ogr2ogr`, `mapshaper` |
| **GeoPackage (.gpkg)** | Modern SQLite-based container for vector + raster data | OGC standard, QGIS default format, government data distributions | `GeoPandas.read_file()`, `fiona`, `ogr2ogr` |

#### Network & Graph Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **Road Network Graphs** | Edge-node graph structures representing transportation networks | OSMnx street networks, HERE road data, TomTom road network | `osmnx` (from OSM), `networkx`, `igraph`, `graph-tool` |
| **Transit / GTFS Feeds** | Standardized public transit schedule and route data | Google GTFS, Transitland, OpenMobilityData, individual transit agencies | `gtfs_functions`, `parade`, `transitland` API |
| **Utility / Infrastructure Networks** | Electrical grids, water distribution, sewer systems | Local utility open data, OSM utility networks, OpenStreetMap power grids | `osmnx`, `GeoPandas`, `overpy` |
| **Flight / Maritime Track Data** | Vessel/aircraft position trajectories over time | AIS (marine), ADS-B (aviation), FlightRadar24, MarineTraffic | `pandas`, `GeoPandas`, `streamz`, `marinetraffic` API |

#### Real-Time & Streaming Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **Weather / Meteorological Feeds** | Real-time weather observations and forecasts | OpenWeatherMap API, MET Norway, NOAA NEXRAD, Weather Underground | `requests`, `siphon`, `pywgrib2`, `xarray` |
| **Traffic / Mobility Feeds** | Live traffic flow, congestion, and vehicle movement | TomTom Traffic, HERE Traffic, Google Traffic, Uber Movement | `requests`, `tomtom` SDK, `herepy` |
| **Seismic / Natural Hazard Feeds** | Real-time earthquake, volcanic, and tsunami alerts | USGS Earthquake API, EMSC, NOAA tsunami warnings | `requests`, `obspy`, `libcomcat` |
| **Social Media / Geotagged Content** | Location-tagged posts, photos, check-ins | Twitter/X API, Flickr API, Instagram (location), Yelp | `tweepy`, `flickrapi`, `instaloader` |
| **IoT Sensor Networks** | Distributed sensor readings with geolocation | Air quality sensors, water level gauges, smart city IoT | MQTT + `paho-mqtt`, `InfluxDB`, `Telegraf` |

#### 3D & Volumetric Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **3D Building Models (BIM/CityGML)** | Semantic 3D building representations at various LODs | CityGML datasets (Cologne, Berlin), BIM IFC files, 3D Tiles | `py3dtiles`, `ifcopenshell`, `3DTiles` loaders |
| **Subsurface / Geology Models** | Volumetric representations of geological layers | Geological survey cross-sections, borehole data, 3D geological maps | `GemPy`, `gsv3d`, `Vulcan`, `Leapfrog` |
| **Ocean / Atmospheric Volumes** | 3D gridded data for water columns or atmospheric layers | HYCOM (ocean), ERA5 (atmosphere), ROMS (regional ocean), WRF (weather) | `xarray`, `netCDF4`, `opendap`, `cfgrib` |

#### Derived / Processed Datasets
| Dataset Type | Description | Examples & Sources | Extraction Methods |
|---|---|---|---|
| **Isochrone Maps** | Polygons representing areas reachable within travel time | OpenRouteService isochrones, Valhalla isochrones, Mapbox Isochrone API | `openrouteservice`, `valhalla` API, `ORS` Python |
| **Heatmaps / Density Surfaces** | Kernel density surfaces showing concentration of phenomena | Crime density, accident density, air pollution concentration | `scipy.stats.gaussian_kde`, `GeoPandas`, `kepler.gl` |
| **Spatial Autocorrelation Indices** | Statistically derived clusters and outliers | LISA clusters, Getis-Ord Gi* hot spots, Moran's I | `PySAL`, `esda`, `splot` |
| **Accessibility / Opportunity Surfaces** | Surfaces quantifying access to services (jobs, healthcare, food) | Walk Score, Opportunity Insights, census-derived accessibility | `pandana`, `OSMnx` + network analysis, `r5py` |
| **Change Maps** | Difference/transition surfaces between time periods | NDVI change, urban expansion masks, deforestation alerts | `rasterio`, `Google Earth Engine`, `eo-learn` |

### Simplification & Smoothing
* **Douglas-Peucker Algorithm**: A line simplification algorithm that reduces the number of points in a complex curve or polygon while preserving its overall shape. Essential for rendering large, complex GeoJSON borders efficiently at lower zoom levels.
* **Chaikin's Algorithm**: A corner-cutting algorithm used to generate smooth curves from a jagged polyline or polygon.

### Space Partitioning & Tessellation
* **Voronoi Diagrams**: An algorithm that partitions a plane into regions based on distance to a specific set of points. Any location within a Voronoi cell is closer to that cell's center point than to any other. Highly useful for generating catchment areas or nearest-neighbor territories.
* **Delaunay Triangulation**: The dual graph of a Voronoi diagram. It connects a set of points into a mesh of triangles such that no point is inside the circumcircle of any triangle. Used for generating 3D terrain meshes (TINs) from scatter points.

### Spatial Indexing & Tree Structures
* **R-Trees**: A tree data structure used for spatial access methods. It groups nearby objects and represents them with their minimum bounding rectangles (MBR). Crucial for quickly querying "what features are visible on the screen right now" without iterating over millions of points. (e.g., `RBush` in JavaScript).
* **K-D Trees**: A space-partitioning data structure for organizing points in a k-dimensional space. Excellent for blazing-fast *nearest-neighbor* searches.
* **Geohashing & Space-Filling Curves**: Algorithms (like Z-order curves or Hilbert curves) that reduce 2D spatial coordinates into a 1D string or integer. This allows databases to perform incredibly fast proximity searches using standard string prefix matching. Modern alternatives include Uber's **H3** (Hexagonal hierarchical spatial index) and Google's **S2** (Spherical geometry).

---

## Global Geospatial Ecosystem (Across Languages)

Beyond the specific web examples listed above, the broader geospatial ecosystem spans almost every major programming language. Here is a comprehensive overview of the most popular mapping, spatial analysis, and geo-data processing libraries available today:

### 🌐 JavaScript / TypeScript (Web & Node.js)
* **Web Mapping**: [Leaflet](https://leafletjs.com/), [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/), [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/), [OpenLayers](https://openlayers.org/)
* **Spatial Analysis**: [Turf.js](https://turfjs.org/) (Advanced geospatial analysis for browsers and Node), [JSTS](https://github.com/bjornharrtell/jsts)

### 🐍 Python
* **Data Analysis & Processing**: [GeoPandas](https://geopandas.org/) (Extends pandas for spatial data), [Shapely](https://shapely.readthedocs.io/) (Geometry manipulation), [Fiona](https://fiona.readthedocs.io/) (Data reading/writing), [Rasterio](https://rasterio.readthedocs.io/) (Raster data)
* **Interactive Mapping**: [Folium](https://python-visualization.github.io/folium/) (Python wrapper for Leaflet)
* **Visualization**: [Cartopy](https://scitools.org.uk/cartopy/docs/latest/), [Geoplot](https://residentmario.github.io/geoplot/)
* **Advanced**: [PySAL](https://pysal.org/) (Spatial analysis library), [OSMnx](https://osmnx.readthedocs.io/) (Street network analysis), [Pyproj](https://pyproj4.github.io/pyproj/) (Projections)

### 📊 R
* **Core Spatial**: [sf](https://r-spatial.github.io/sf/) (Simple Features), [sp](https://cran.r-project.org/web/packages/sp/) (Legacy spatial classes)
* **Raster Data**: [terra](https://rspatial.github.io/terra/), [raster](https://cran.r-project.org/web/packages/raster/)
* **Mapping**: [tmap](https://github.com/r-tmap/tmap) (Thematic maps), [leaflet](https://rstudio.github.io/leaflet/) (R interface to Leaflet), [mapview](https://r-spatial.github.io/mapview/)

### ☕ Java / JVM
* **Core Libraries**: [GeoTools](https://geotools.org/) (Open source Java GIS toolkit), [JTS Topology Suite](https://github.com/locationtech/jts) (The gold standard for geometry operations)
* **Standards & Processing**: [Apache SIS](https://sis.apache.org/) (Spatial Information System)
* **Routing**: [GraphHopper](https://www.graphhopper.com/) (Fast routing engine)
* **Spatial Search**: [Spatial4j](https://github.com/locationtech/spatial4j)

### ⚙️ C / C++
* **Foundational GIS Libraries**: [GDAL/OGR](https://gdal.org/) (Geospatial Data Abstraction Library - powers almost everything), [GEOS](https://libgeos.org/) (C++ port of JTS)
* **Projections**: [PROJ](https://proj.org/) (Coordinate transformation software)
* **Rendering & Routing**: [Mapnik](https://mapnik.org/) (Map rendering toolkit), [OSRM](http://project-osrm.org/) (Open Source Routing Machine)

### 🐹 Go (Golang)
* **Geometry**: [orb](https://github.com/paulmach/orb) (2D geometry types), [go-spatial/geom](https://github.com/go-spatial/geom)
* **Databases**: [Tile38](https://tile38.com/) (In-memory geolocation data store and spatial index)

### 🦀 Rust
* **Geospatial Core**: [geo](https://github.com/georust/geo) (Geospatial primitives and algorithms), [geo-types](https://crates.io/crates/geo-types)
* **Bindings**: [gdal](https://crates.io/crates/gdal) (Rust bindings for GDAL), [proj](https://crates.io/crates/proj)
* **Indexing**: [h3-rs](https://github.com/rusty-celery/h3-rs) (Uber's H3 hierarchical indexing)

### 📱 Mobile (iOS & Android)
* **Native Maps**: Apple MapKit (iOS), Google Maps SDK (iOS/Android)
* **Open Source / Custom**: [Mapbox Maps SDK](https://docs.mapbox.com/ios/maps/guides/), [OSMDroid](https://github.com/osmdroid/osmdroid) (Android replacement for Google Maps), [CARTO Mobile SDK](https://carto.com/)

### 🗄️ Geospatial Databases & SQL Extensions

To build scalable map applications (like dynamic store locators, fleet tracking, or massive data analysis), you need a database that understands geometry. Here are the standard database plugins and tools for geospatial operations:

#### Relational Databases (SQL)
* **PostgreSQL + [PostGIS](https://postgis.net/)**: The undisputed gold standard for geospatial databases. PostGIS transforms PostgreSQL into a powerhouse spatial database, adding `geometry`/`geography` data types and hundreds of spatial functions (e.g., `ST_Intersects`, `ST_Buffer`, `ST_Distance`).
* **PostgreSQL + [pgRouting](https://pgrouting.org/)**: An extension built on top of PostGIS that provides graph-based geospatial routing algorithms (Dijkstra, A*, Isochrones) directly in the database.
* **SQLite + [SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite/index)**: A robust spatial extension for SQLite. It provides PostGIS-like functionality but in a lightweight, serverless file format. Highly popular for offline mobile map apps (iOS/Android) and portable desktop GIS workflows.
* **DuckDB + [duckdb_spatial](https://duckdb.org/docs/extensions/spatial.html)**: An extremely fast extension for DuckDB designed for geospatial analytics. Perfect for querying massive datasets in Parquet, CSV, or GeoJSON formats locally without setting up a server.

#### NoSQL, Search & In-Memory
* **[MongoDB](https://www.mongodb.com/docs/manual/geospatial-queries/)**: Provides first-class, native support for storing GeoJSON objects. Out-of-the-box, you can easily create 2dsphere indexes and run `$geoIntersects` or `$near` queries for location-based apps.
* **[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-point.html)**: Excellent for spatial search at scale. Provides `geo_point` and `geo_shape` data types for lightning-fast bounding-box and polygon intersection queries alongside full-text search.
* **[Redis GEO](https://redis.io/docs/data-types/geospatial/)**: Built-in commands (`GEOADD`, `GEORADIUS`, `GEOSEARCH`) for blazing fast, in-memory location-based caching and proximity queries (e.g., finding nearby drivers).
* **[Tile38](https://tile38.com/)**: A dedicated, ultra-fast in-memory spatial database and geofencing server. Excels at live fleet tracking, delivering real-time webhooks when moving objects enter or exit complex polygons.

---

## Libraries by Language

A comprehensive reference of maps/geo libraries across every major programming language.

### JavaScript / TypeScript

#### Interactive Map Rendering
- Library - Description
- [Leaflet](https://leafletjs.com/) - Mobile-friendly interactive maps with raster tiles
- [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/) - WebGL vector map rendering (fork of Mapbox GL JS)
- [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/) - WebGL-powered vector maps (proprietary)
- [OpenLayers](https://openlayers.org/) - High-performance maps with raster + vector support
- [Leaflet.VectorGrid](https://github.com/Leaflet/Leaflet.VectorGrid) - Efficient vector tile rendering for Leaflet
- [ESRI ArcGIS JS API](https://developers.arcgis.com/javascript/) - Enterprise GIS mapping SDK
- [Google Maps JavaScript API](https://developers.google.com/maps/documentation/javascript) - Google Maps embedding and interaction
- [Here Maps JS API](https://developer.here.com/documentation/javascript-api) - HERE interactive map embedding

#### Map Component Frameworks
- Library - Description
---

### Python

#### Interactive Maps
- Library - Description
- [Folium](https://python-visualization.github.io/folium/) - Leaflet maps in Python (Jupyter friendly)
- [Kepler.gl](https://kepler.gl/) - Geospatial data exploration (Jupyter)
- [IPyleaflet](https://github.com/jupyter-widgets/ipyleaflet) - Leaflet for Jupyter widgets
- [Mapwidget](https://github.com/innogeo/mapwidget) - Interactive map widgets for Jupyter

#### Geospatial Analysis (Core)
- Library - Description
- [GeoPandas](https://geopandas.org/) - Spatial dataframes (Pandas + geometry)
- [Shapely](https://shapely.readthedocs.io/) - Geometric operations (buffer, union, intersection)
- [Fiona](https://fiona.readthedocs.io/) - OGR vector data I/O
- [Rasterio](https://rasterio.readthedocs.io/) - GDAL-based raster I/O
- [GDAL/Python bindings](https://gdal.org/api/python/) - Raster + vector format translation
- [pyproj](https://pyproj4.github.io/pyproj/) - Coordinate transformations (PROJ wrapper)
- [Rtree](https://rtree.readthedocs.io/) - Spatial indexing (R-tree via libspatialindex)
- [PySAL](https://pysal.org/) - Spatial analysis (hotspots, autocorrelation, regression)
- [tobler](https://pysal.org/tobler/) - Areal interpolation
- [mgwr](https://pysal.org/mgwr/) - Geographically weighted regression

#### Routing & Network
- Library - Description
- [OSRM Python bindings](https://github.com/Project-OSRM/osrm-backend) - OSRM routing engine
- [Valhalla Python](https://github.com/valhalla/valhalla) - Valhalla routing engine bindings
- [networkx](https://networkx.org/) - Graph algorithms (used with OSMnx)
- [OSMnx](https://github.com/gboeing/osmnx) - Download + model street networks from OSM
- [Pandana](https://github.com/UDST/pandana) - Urban network analysis

#### Geocoding
- Library - Description
- [geopy](https://geopy.readthedocs.io/) - Geocoding (Nominatim, Google, Bing, etc.)
- [pgeocode](https://github.com/sytelus/pgeocode) - Postal code geocoding
- [geopandas.tools.sjoin](https://geopandas.org/) - Spatial joins

#### OSM Data Processing
- Library - Description
- [PyOsmium](http://osmcode.org/pyosmium/) - PBF/XML processing toolkit
- [osm2geojson](https://github.com/aspectumapp/osm2geojson) - OSM XML/Overpass to GeoJSON
- [overpy](https://pypi.org/project/overpy/) - Overpass API client
- [osmread](https://github.com/dezhin/osmread) - XML and PBF reader
- [imposm](https://github.com/omniscale/imposm3) - OSM data into PostGIS

#### Tile Serving & Rendering
- Library - Description
- [Tileserver GL](https://github.com/mapbox/tileserver-gl) - Vector tile serving
- [Martin](https://github.com/maplibre/martin) - PostGIS/MBTiles vector tile server
- [Kartograph](https://github.com/kartograph/kartograph.py) - Map rendering framework

---

### Java / Kotlin

#### Interactive Maps
- Library - Description
- [osmdroid](https://github.com/osmdroid/osmdroid) - Android map view (OSM-based, replaces MapView v1)
- [OSMBonusPack](https://github.com/osmdroid/OSMBonusPack) - Markers, bubbles, routes, KML for osmdroid
- [JXMapViewer2](https://github.com/petr-panteleyev/jxmapviewer2) - Swing map widget
- [JMapViewer](https://wiki.openstreetmap.org/wiki/JMapViewer) - Java SE map viewer
- [OpenMap](https://openmap.org/) - Java Swing/Java2D mapping toolkit
- [Geotools](https://geotools.org/) - Java GIS toolkit (OGC compliant)
- [GeoTools MapPane](https://docs.geotools.org/) - Swing map rendering component

#### Native Map Rendering
- Library - Description
- [MapLibre Native](https://github.com/maplibre/maplibre-native) - Cross-platform vector/raster tile rendering (Android/iOS/desktop)
- [Tangram ES](https://github.com/tangrams/tangram-es) - OpenGL ES 2D/3D map renderer
- [mapsforge](https://github.com/mapsforge/mapsforge) - Android offline map rendering (OSM data)
- [CartoType](https://cartotype.github.io/cartotype/) - Offline vector map rendering + routing
- [libosmscout](https://github.com/Framstag/libosmscout) - Offline vector map rendering + routing
- [OSMSharp](https://github.com/xivk/osmsharp) - Offline vector map rendering (.NET/Java)
- [GLMap](https://github.com/nickaknudson/android-glmap) - OpenGL ES vector map (Android/iOS)

#### Routing & Navigation
- Library - Description
- [GraphHopper](https://www.graphhopper.com/) - Java routing engine (car, bike, foot)
- [OSRM Java](https://github.com/Project-OSRM/osrm-backend) - OSRM bindings
- [Valhalla](https://github.com/valhalla/valhalla) - C++ routing engine (Java JNI bindings)
- [OpenTripPlanner](https://www.opentripplanner.org/) - Multimodal transit routing
- [Mapbox Navigation Android](https://github.com/mapbox/mapbox-navigation-android) - Turn-by-turn navigation UI
- [MapLibre Navigation Android](https://github.com/maplibre/maplibre-navigation-android) - Turn-by-turn navigation UI

#### Geocoding
- Library - Description
- [Gisgraphy](http://www.gisgraphy.com/) - Java geocoding/reverse geocoding server
- [osm-common](https://github.com/kodapan/osm-common/) - Java OSM API + Nominatim client
- [osmapi](https://github.com/westnordost/osmapi/) - Complete OSM API 0.6 implementation (Java)
- [Pelias Android SDK](https://github.com/pelias/pelias-android-sdk/) - Pelias geocoder for Android

#### OSM Data Processing
- Library - Description
- [libosmium](https://github.com/osmcode/libosmium) - C++ toolkit for OSM data (Java wrappers exist)
- [osm4j](https://github.com/topobyte/osm4j) - Java PBF/XML reader
- [Osmosis](https://github.com/openstreetmap/osmosis) - Java OSM data processing pipeline
- [Atlas](https://github.com/osmlab/atlas) - OSM data integrity checks at scale (Spark)
- [GeoDesk](https://geodesk.com/) - Fast OSM data engine (Java/C++)
- [OSMonaut](https://github.com/MorbZ/OSMonaut/) - Binary OSM file parser
- [parallelpbf](https://github.com/woltapp/parallelpbf) - Multithreaded PBF reader

#### Tile Rendering
- Library - Description
- [Mapnik](https://mapnik.org/) - Server-side 2D map renderer (Python/C++ bindings)
- [TileServer GL](https://github.com/mapbox/tileserver-gl) - Vector tile server (Node.js)
- [pgmapcss](https://github.com/gravitystorm/openstreetmap-carto) - PostgreSQL/PostGIS map styling

#### GIS Libraries
- Library - Description
- [GeoTools](https://geotools.org/) - Java GIS toolkit (OGC standards)
- [JTS](https://locationtech.github.io/jts/) - Java Topology Suite (geometry operations)
- [GeoAPI](https://geoapi.org/) - Java OGC API interfaces

---

### Swift / Objective-C (iOS / macOS)

#### Interactive Maps
- Library - Description
- [Mapbox iOS SDK](https://github.com/mapbox/mapbox-gl-native-ios) - Vector tile rendering with OpenGL ES
- [MapLibre Native iOS](https://github.com/maplibre/maplibre-native) - Open-source fork of Mapbox iOS SDK
- [Apple MapKit](https://developer.apple.com/mapkit/) - Native Apple Maps embedding
- [CartoType iOS](https://cartotype.github.io/cartotype/) - Offline vector maps + routing

#### Routing & Navigation
- Library - Description
- [Mapbox Directions for Swift](https://github.com/mapbox/mapbox-directions-swift) - Directions API client
- [Mapbox Navigation iOS](https://github.com/mapbox/mapbox-navigation-ios) - Turn-by-turn navigation UI
- [MapLibre Navigation iOS](https://github.com/maplibre/maplibre-navigation-ios) - Turn-by-turn navigation UI
- [GraphHopper iOS](https://github.com/graphhopper/directions-api) - GraphHopper routing client

#### Geocoding
- Library - Description
- [Mapbox Geocoder for Swift](https://github.com/mapbox/mapboxgeocoder.swift) - Geocoding client
- [Pelias iOS SDK](https://github.com/pelias/pelias-ios-sdk/) - Pelias geocoder for iOS

#### OSM Data
- Library - Description
- [OSMKit](https://github.com/davidchiles/OSMKit) - PBF/XML parser + Spatialite storage

---

### C# / .NET

#### Interactive Maps
- Library - Description
- [Mapsui](https://github.com/Mapsui/Mapsui) - Cross-platform map control (Xamarin/MAUI/Avalonia)
- [BruTile](https://github.com/BruTile/BruTile) - Tile source abstraction
- [Carto Mobile SDK](https://github.com/CartoDB/mobile-sdk/) - Cross-platform map SDK
- [Xamarin.Forms.GoogleMaps](https://github.com/amay077/Xamarin.Forms.GoogleMaps) - Google Maps for Xamarin.Forms
- [Mapbox SDK for Xamarin](https://github.com/nickaknudson/xamarin-mapbox) - Mapbox for Xamarin

#### Geospatial Analysis
- Library - Description
- [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) - JTS port (geometry operations)
- [ProjNET](https://github.com/NetTopologySuite/ProjNet) - Coordinate transformations
- [GeoAPI](https://github.com/NetTopologySuite/GeoAPI) - OGC interfaces for .NET
- [SharpMap](https://github.com/SharpMap/SharpMap) - GIS mapping engine
- [DotSpatial](https://github.com/DotSpatial/DotSpatial) - GIS library for .NET

#### Routing
- Library - Description
- [Itinero](https://github.com/itinero/routing) - Open-source .NET routing engine
- [OSRM .NET](https://github.com/pgoettler/osrm-net) - OSRM bindings

#### Geocoding
- Library - Description
- [Nominatim.API](https://github.com/f1ana/Nominatim.API) - Nominatim client for .NET
- [OsmApiClient](https://github.com/blackboxlogic/OsmApiClient) - OSM API v0.6 client

#### OSM Data
- Library - Description
- [OsmSharp](https://github.com/xivk/osmsharp) - OSM data processing + offline rendering
- [OSMJSON.Net](https://github.com/FaFre/OSMJSON.Net) - OSM JSON serialization
- [OsmTagsTranslator](https://github.com/blackboxlogic/OsmTagsTranslator) - SQLite-based tag translation
- [OsmStream](https://github.com/blackboxlogic/OsmStream) - Stream-based OSM data processing

---

### C / C++

#### Map Rendering
- Library - Description
- [Mapnik](https://mapnik.org/) - Industry-standard 2D map renderer
- [MapLibre Native](https://github.com/maplibre/maplibre-native) - Cross-platform vector/raster tile rendering
- [Tangram ES](https://github.com/tangrams/tangram-es) - OpenGL ES 2D/3D map renderer
- [libosmscout](https://github.com/Framstag/libosmscout) - Offline vector maps + routing
- [CartoType](https://cartotype.github.io/cartotype/) - Offline vector maps + routing

#### Geospatial Libraries
- Library - Description
- [GDAL](https://gdal.org/) - Raster + vector geospatial data abstraction
- [OGR](https://gdal.org/ogr/) - Vector data processing (part of GDAL)
- [GEOS](https://libgeos.org/) - Geometry engine (JTS C++ port)
- [PROJ](https://proj.org/) - Coordinate transformations
- [SpatiaLite](https://www.gaia-gis.it/gaia-sins/) - Spatial SQL extension for SQLite
- [libspatialindex](https://github.com/libspatialindex/libspatialindex) - R-tree spatial indexing
- [libosmium](https://github.com/osmcode/libosmium) - Fast OSM data processing toolkit
- [Boost.Geometry](https://www.boost.org/doc/libs/1_85_0/libs/geometry/doc/index.html) - Geometric algorithms in Boost

#### Routing
- Library - Description
- [OSRM](https://github.com/Project-OSRM/osrm-backend) - Open-source routing machine
- [Valhalla](https://github.com/valhalla/valhalla) - Open-source routing engine
- [GraphHopper](https://github.com/graphhopper/graphhopper) - Java routing engine (C++ core)
- [Routino](https://github.com/routino/routino) - Flexible OSM router

#### Tile & Data Formats
- Library - Description
- [libprotobuf](https://github.com/protocolbuffers/protobuf) - Protocol Buffers (used by vector tiles)
- [protozero](https://github.com/mapbox/protozero) - Minimal protobuf decoder (vector tiles)
- [Mapbox Vector Tile spec](https://github.com/mapbox/vector-tile-spec) - MVT format reference

---

### Rust

#### Geospatial Libraries
- Library - Description
- [geo](https://github.com/georust/geo) - Geospatial primitives + algorithms
- [geos](https://github.com/georust/geos) - GEOS bindings (geometry operations)
- [proj](https://github.com/georust/proj) - PROJ bindings (coordinate transforms)
- [geo-types](https://github.com/georust/geo-types) - Core geometry types
- [geojson](https://github.com/georust/geojson) - GeoJSON parsing/serialization
- [GDAL bindings](https://github.com/georust/gdal) - GDAL Rust bindings
- [kdtree](https://github.com/mneumark/kdtree) - K-D tree spatial indexing
- [rstar](https://github.com/georust/rstar) - R-tree spatial indexing

#### Routing
- Library - Description
- [Valhalla](https://github.com/valhalla/valhalla) - C++ routing engine (Rust bindings exist)
- [osrm-backend](https://github.com/Project-OSRM/osrm-backend) - C++ routing (Rust bindings exist)

#### OSM Data
- Library - Description
- [osm-io](https://github.com/nickaknudson/osm-io) - Fast PBF read/write
- [osm-admin](https://github.com/nickaknudson/osm-admin) - PBF import/export to PostGIS

#### Tile Serving
- Library - Description
- [Martin](https://github.com/maplibre/martin) - PostGIS/MBTiles vector tile server
- [tileserver](https://github.com/maplibre/tileserver) - Tile serving (Rust)

---

### Go

#### Geospatial Libraries
- Library - Description
- [geom](https://github.com/paulmach/geom) - Geometric types + operations
- [s2](https://github.com/golang/geo) - S2 geometry library (Google)
- [H3-go](https://github.com/uber/h3-go) - H3 hexagonal hierarchical spatial index
- [go-geoindex](https://github.com/kellydunn/golang-geo) - Geospatial indexing
- [go-proj](https://github.com/uber/go-proj-4) - PROJ.4 bindings

#### OSM Data
- Library - Description
- [gOSMonaut](https://github.com/MorbZ/gosmonaut) - PBF parser (nested entities)
- [gosmparse](https://github.com/thomersch/gosmparse) - High-speed PBF parser
- [osmpbf](https://github.com/qedus/osmpbf) - PBF format parser
- [tdewolff/geo](https://github.com/tdewolff/geo/tree/master/osm) - High-performance PBF parser + geometry extractor
- [go-osm](https://github.com/paulmach/go.osm) - OSM data structures

#### Routing
- Library - Description
- [go-osrm](https://github.com/paulmach/go-osrm) - OSRM API client
- [graphhopper](https://github.com/graphhopper/graphhopper) - Go routing engine port

---

---

### Ruby

#### Geospatial Libraries
- Library - Description
- [Rosemary](https://github.com/sozialhelden/rosemary/) - OSM API client
- [OSMLib](https://github.com/pnorman/OSMLib) - OSM to shapefile conversion
- [rgeo](https://github.com/rgeo/rgeo) - Geospatial library (GEOS bindings)
- [rgeo-geojson](https://github.com/rgeo/rgeo-geojson) - GeoJSON parsing
- [rspatial](https://github.com/rspatial) - Spatial analysis

---

### PHP

#### Geospatial Libraries
- Library - Description
- [Services_OpenStreetMap](https://github.com/pear/Services_Openstreetmap/) - OSM API client (PEAR)
- [League\Geotools](https://github.com/league/geotools) - Geocoding, distance, bounding box
- [GeoPHP](https://github.com/phayes/geoPHP) - Geometry library (WKT, GeoJSON, etc.)

---

### R

#### Geospatial Libraries
- Library - Description
- [sf](https://r-spatial.github.io/sf/) - Simple Features for R (vector data)
- [terra](https://rspatial.org/terra/) - Spatial raster data
- [stars](https://r-spatial.github.io/stars/) - Spatiotemporal arrays
- [sp](https://cran.r-project.org/package=sp) - Spatial data classes (legacy)
- [leaflet](https://rstudio.github.io/leaflet/) - Leaflet for R
- [tmap](https://r-tmap.github.io/tmap/) - Thematic map visualization
- [osmdata](https://github.com/ropensci/osmdata) - Download OSM data via Overpass API
- [osmapiR](https://github.com/mab68/osmapiR) - OSM API client
- [osmaR](http://osmar.r-forge.r-project.org/) - OSM API client
- [OpenRouteService](https://github.com/GIScience/openrouteservice-r) - ORS routing client

---

### Scala

#### Geospatial Libraries
- Library - Description
- [geotrellis](https://geotrellis.io/) - Geospatial data processing (raster + vector)
- [osm4scala](https://github.com/angelcervera/osm4scala/) - High-performance PBF parser
- [geow](https://github.com/plasmap/geow/) - Lightweight OSM data processing
- [spark-osm-datasource](https://github.com/woltapp/spark-osm-datasource) - PBF for Apache Spark

---

### Other Languages

#### Lua
- Library - Description
- [TileMan](https://github.com/osmfj/tileman/) - OSM tile serving framework

#### Objective-C++
- Library - Description
- [CartoType](https://cartotype.github.io/cartotype/) - Offline vector maps (iOS/Android/C++)

#### Haskell
- Library - Description
- [haskell-gis](https://hackage.haskell.org/package/gis) - GIS data structures

#### Julia
- Library - Description
- [ArchGDAL.jl](https://github.com/JuliaGeo/ArchGDAL.jl) - GDAL bindings
- [GeoInterface.jl](https://github.com/JuliaGeo/GeoInterface.jl) - Geometry interface
- [Meshes.jl](https://github.com/JuliaGeometry/Meshes.jl) - Geometric mesh processing

#### OCaml
- Library - Description
- [Osm.ml](https://github.com/ocaml-documented-stuff/osm.ml) - OSM file parser

---

### Data & Service APIs

#### Tile Services
- Service - Description
- [OpenStreetMap Tiles](https://www.openstreetmap.org/wiki/Tile_servers) - Free raster tile server
- [Protomaps](https://protomaps.com/) - Open-source vector tile hosting
- [MapTiler](https://www.maptiler.com/) - Cloud vector/raster tiles
- [Stadia Maps](https://stadiamaps.com/) - Vector tile hosting
- [Thunderforest](https://thunderforest.com/) - Specialty map tiles
- [Mapbox](https://www.mapbox.com/) - Vector tile platform

#### Geocoding APIs
- Service - Description
- [Nominatim](https://nominatim.org/) - OpenStreetMap forward/reverse geocoder
- [Pelias](https://github.com/pelias/pelias) - Open-source geocoder
- [Photon](https://photon.komoot.io/) - Open-source geocoder (based on OSM)
- [Google Geocoding](https://developers.google.com/maps/documentation/geocoding) - Google geocoding API
- [HERE Geocoding](https://developer.here.com/geocoding-api) - HERE geocoding API

#### Routing APIs
- Service - Description
- [OSRM](http://project-osrm.org/) - Open-source routing machine
- [Valhalla](https://valhalla.github.io/) - Open-source routing engine
- [GraphHopper](https://www.graphhopper.com/) - Routing API
- [OpenRouteService](https://openrouteservice.org/) - Multimodal routing
- [Mapbox Directions](https://docs.mapbox.com/api/navigation/directions/) - Routing API
- [HERE Routing](https://developer.here.com/routing-api) - Routing API
- [Google Directions](https://developers.google.com/maps/documentation/directions) - Routing API

#### Weather APIs
- Service - Description
- [OpenWeatherMap](https://openweathermap.org/) - Weather data + tile overlays
- [Tomorrow.io](https://www.tomorrow.io/) - Weather API
- [MET Norway](https://api.met.no/) - Free weather API

#### Traffic APIs
- Service - Description
- [TomTom Traffic](https://developer.tomtom.com/traffic-api) - Traffic flow + incidents
- [HERE Traffic](https://developer.here.com/traffic-api) - Traffic flow + incidents
- [Azure Maps Traffic](https://docs.microsoft.com/rest/api/maps/traffic) - Traffic flow + incidents

---

## Spatial Databases & Backend Tools

Databases and server-side tools that store, index, and query geospatial data.

### Relational Spatial Databases

| Database | Spatial Engine | Key Features |
|----------|---------------|--------------|
| [PostGIS](https://postgis.net/) | GEOS, PROJ, GDAL | Full-featured spatial SQL: geometry types, spatial indexes (GiST, BRIN), 700+ spatial functions, raster support, topology, vector tiles via pg_tileserv |
| [SQLite + SpatiaLite](https://www.gaia-gis.it/gaia-sins/) | GEOS, PROJ, GDAL | Embedded spatial DB: geometry types, spatial indexes, R-tree, spatial SQL, supports WKT/WKB/GeoJSON |
| [MySQL Spatial](https://dev.mysql.com/doc/refman/8.0/en/spatial.html) | Internal | Geometry types, spatial indexes (R-tree), basic spatial functions (ST_Area, ST_Contains, ST_Distance, etc.) |
| [MariaDB Spatial](https://mariadb.com/kb/en/spatial-data-maria/) | Internal | MySQL-compatible spatial extensions |
| [Oracle Spatial and Graph](https://docs.oracle.com/en/database/oracle/oracle-database/21/spatl/) | SDO_GEOMETRY | Enterprise spatial: vector/raster, topology, network, geocoding, routing |
| [SQL Server Spatial](https://learn.microsoft.com/en-us/sql/relational-databases/spatial/spatial-data-sql-server) | Internal | Geometry + Geography types, spatial indexes, spatial methods (STIntersects, STDistance, etc.) |
| [DB2 Spatial](https://www.ibm.com/docs/en/db2/11.5?topic=concepts-spatial-data) | Internal | Geometry + Geography types, spatial indexes, spatial queries |

### PostgreSQL Extensions & Plugins

| Extension | Purpose | Key Features |
|-----------|---------|--------------|
| [PostGIS](https://postgis.net/) | Spatial database engine | Geometry/Geography types, 700+ spatial functions, spatial indexes (GiST, BRIN, SP-GiST), raster support, topology, vector tiles (ST_AsMVT), 3D geometry, M support |
| [pgRouting](https://pgrouting.org/) | Network routing | Dijkstra, A*, KSP, driving distance, TSP, vehicle routing, TRSP, with-points, contraction hierarchies, pgRouting + PostGIS spatial routing |
| [H3-Pg](https://github.com/bytesandbrains/h3-pg) | Hexagonal hierarchical index | Uber H3 index in SQL, h3_to_geo, geo_to_h3, hex boundary, ring, k-ring, hex distance |
| [pg_h3](https://github.com/bytesandbrains/h3-pg) | H3 binding | H3 functions exposed as PostgreSQL functions |
| [pgpointcloud](https://github.com/pgpointcloud/pointcloud) | Point cloud storage | Point cloud data (LiDAR) in PostgreSQL, PCPoint, PCPatch types, schema-based compression |
| [pg_raster](https://postgis.net/documentation/overview/raster/) | Raster data | GeoTIFF, raster columns, raster bands, raster algebra, ST_MapAlgebra, ST_ZonalStats |
| [pg_topology](https://postgis.net/documentation/overview/topology/) | Topological data | TopoGeometry, TopoElement, CreateTopology, TopoGeo_AddLinestring, valid topology graphs |
| [pg_geohash](https://github.com/kamicase/postgresql-geohash) | Geohashing | Encode/decode geohash, distance, nearby search |
| [PostGIS SFCGAL](https://postgis.net/documentation/reference/#sfcgal) | 3D geometry operations | 3D union, intersection, difference, extrude, TIN generation, straight skeleton, minkowski sum |
| [pg_analytics](https://github.com/paradedb/paradedb) | Analytics on PostGIS | ParadeDB analytics functions for PostGIS data |
| [pg_sodium](https://github.com/michelp/pgsodium) | Encryption | Transparent column encryption (not spatial, but used with geo data for security) |
| [Citus](https://www.citusdata.com/) | Distributed PostgreSQL | Distributed spatial queries across nodes, sharding for large PostGIS datasets |
| [TimescaleDB](https://www.timescale.com/) | Time-series + PostGIS | Hypertables for spatiotemporal data, time-bucket with spatial aggregation |
| [pg_partman](https://github.com/pgpartman/pg_partman) | Partitioning | Table partitioning for large spatial tables (by region, time, etc.) |
| [PostGIS Tiger Geocoder](https://postgis.net/documentation/overview/tiger_geocoder/) | Geocoding | US address geocoding using TIGER/Line data, geocode(), reverse_geocode() |
| [pg_featureserv](https://github.com/CrunchyData/pg_featureserv) | OGC API Features | RESTful feature service from PostGIS tables, OGC API compliant |
| [pg_tileserv](https://github.com/CrunchyData/pg_tileserv) | Vector tile server | Serve MVT vector tiles directly from PostGIS |
| [pg_routing](https://pgrouting.org/) | Routing functions | Shortest path, driving distance, isochrone, TSP, VRP |
| [pg_labelmap](https://github.com/pgvector/pgvector) | Vector search | (pgvector) Vector similarity search for geospatial embeddings |

### SQLite Extensions & Plugins

| Extension | Purpose | Key Features |
|-----------|---------|--------------|
| [SpatiaLite](https://www.gaia-gis.it/gaia-sins/) | Spatial database engine | Full spatial SQL for SQLite: geometry types, 700+ spatial functions, R-tree, spatial indexes, KML/GeoJSON/WKT export, topology support |
| [mod_spatialite](https://www.gaia-gis.it/gaia-sins/) | SpatiaLite loadable module | ModSpatialite: spatial functions as loadable SQLite extension, PROJ, GEOS, GDAL integration |
| [RTree (built-in)](https://www.sqlite.org/rtree.html) | Spatial indexing | Native SQLite R-tree module, 2D bounding box indexing, fast spatial queries |
| [SQLite R*Tree](https://www.sqlite.org/rtree.html) | R-tree index | Prefix/range/constraint queries on multi-dimensional data |
| [geo_module](https://github.com/nickolay/geo_module) | GeoJSON support | GeoJSON reading/writing for SQLite |
| [sqlite-geopoly](https://www.sqlite.org/geopoly.html) | Polygon overlap | Polyfill module for polygon overlap queries, virtual table support |
| [spatialite-tools](https://www.gaia-gis.it/gaia-sins/spatialite-tools.html) | CLI tools | shp2sqlite, sqlite2shp, spatialite_sql, import/export utilities |
| [ogr_fdw](https://github.com/pramsey/pgsql-ogr-fdw) | Foreign data wrapper | (PostgreSQL) OGR-based FDW to query SQLite/SpatiaLite files as foreign tables |
| [GDAL SQLite driver](https://gdal.org/en/stable/drivers/vector/sqlite.html) | Data access | Read/write SQLite databases with OGR, supports SpatiaLite geometry |
| [SQLite Cloud](https://sqlitecloud.io/) | Cloud SQLite | Cloud-hosted SQLite with SpatiaLite support |
| [libspatialite](https://www.gaia-gis.it/gaia-sins/) | Core library | The C library powering SpatiaLite, GEOS/PROJ/GDAL integration |
| [DuckDB Spatial](https://duckdb.org/docs/extensions/spatial.html) | Embedded analytics | Spatial extension for DuckDB (SQLite-like), geometry types, spatial functions, GeoJSON/Shapefile I/O |
| [Kine](https://github.com/kinecosystem/kin-core) | Key-value store | (not spatial) Reference for embedding patterns |
| [sqlite-vss](https://github.com/asg017/sqlite-vss) | Vector search | Vector similarity search for SQLite (useful for geospatial embeddings) |
| [sqlite-vec](https://github.com/asg017/sqlite-vec) | Vector search | Pure C vector search extension for SQLite |
| [sqlite3_spatialite](https://www.gaia-gis.it/gaia-sins/) | Precompiled builds | Ready-to-use SpatiaLite binaries for multiple platforms |

### SpatiaLite Spatial Functions (Key Operations)

| Category | Functions |
|----------|-----------|
| **Measurement** | `ST_Area`, `ST_Length`, `ST_Perimeter`, `ST_Distance`, `ST_LineLength`, `ST_PolygonLength` |
| **Relationships** | `ST_Contains`, `ST_Within`, `ST_Intersects`, `ST_Crosses`, `ST_Touches`, `ST_Disjoint`, `ST_Overlaps`, `ST_Equals` |
| **Set Operations** | `ST_Union`, `ST_Intersection`, `ST_Difference`, `ST_SymDifference`, `ST_Collect` |
| **Overlay** | `ST_Buffer`, `ST_Simplify`, `ST_SimplifyPreserveTopology`, `ST_MakeValid`, `ST_Centroid` |
| **Conversion** | `ST_GeomFromText`, `ST_GeomFromGeoJSON`, `ST_AsGeoJSON`, `ST_AsKML`, `ST_AsSVG` |
| **Transformation** | `ST_Transform`, `ST_SetSRID`, `ST_CoordDim`, `ST_Z` |
| **Indexing** | `RTree` (built-in), `CreateSpatialIndex`, `ST_MakeEnvelope`, bounding box operators |
| **Validation** | `ST_IsValid`, `ST_MakeValid`, `ST_IsEmpty`, `ST_IsSimple` |
| **Topology** | `CreateTopology`, `TopoGeo_GeomFromText`, `ST_GetFaceBoundary`, `ST_ModFace` |
| **TIN** | `ST_Triangulate2DZ`, `ST_DelaunayTriangles`, `ST_TIN` |
| **Grid** | `GenerateSpatialGrid`, `GeneratePoints`, `HexagonalGrid` |

### PostGIS Extension Comparison

| Feature | PostGIS | SpatiaLite | MySQL Spatial | Oracle Spatial |
|---------|---------|------------|---------------|----------------|
| Geometry types | Yes | Yes | Yes | Yes |
| Geography (spherical) | Yes | Yes (limited) | No | Yes |
| Spatial indexes | GiST, BRIN, SP-GiST, Hash | R-tree | R-tree | R-tree |
| Raster support | Yes (raster columns) | No | No | Yes |
| Topology | Yes (topology module) | Yes | No | Yes |
| 3D geometry | Yes | Yes | No | Yes |
| M (measure) values | Yes | Yes | No | Yes |
| Vector tiles (MVT) | Yes (ST_AsMVT) | No | No | No |
| Routing | pgRouting extension | No | No | Network extension |
| Point clouds | pgpointcloud extension | No | No | Yes |
| Geocoding | Tiger Geocoder | No | No | Yes |
| OGC API | pg_featureserv | No | No | Yes |
| JSON export | ST_AsGeoJSON | ST_AsGeoJSON | ST_AsGeoJSON | SDO_UTIL.TO_GEOJSON |
| Loadable module | No (shared lib) | Yes (mod_spatialite) | No | No |
| Embedded | No | Yes (SQLite) | No | No |

### NoSQL & Document Spatial Databases

| Database | Spatial Support | Key Features |
|----------|----------------|--------------|
| [MongoDB](https://www.mongodb.com/docs/manual/geospatial-queries/) | 2dsphere index | GeoJSON storage, $geoNear, $geoWithin, $geoIntersects queries, geospatial aggregation |
| [Redis + RediSearch](https://redis.io/docs/interact/search-and-query/) | GEO commands | GEOADD, GEODIST, GEORADIUS, GESEARCH, geospatial indexing |
| [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo.html) | geo_shape, geo_point | Spatial queries (contains, within, intersects), bounding box, geohash, distance sorting |
| [CouchDB + GeoMesa](https://couchdb.apache.org/) | Spatial views | GeoJSON storage, spatial Mango queries |
| [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GeoLibrary.html) | Geo library | Geohash-based spatial queries, GeoPoint data type |
| [FoundationDB](https://apple.github.io/foundationdb/) | Layer-based | Record layer with geospatial indexing support |

### Vector Tile Databases & Servers

| Tool | Description |
|------|-------------|
| [Martin](https://github.com/maplibre/martin) | PostGIS/MBTiles/PMTiles vector tile server (Rust) |
| [pg_tileserv](https://github.com/CrunchyData/pg_tileserv) | PostGIS-native vector tile server |
| [pg_featureserv](https://github.com/CrunchyData/pg_featureserv) | PostGIS-native feature service (OGC API) |
| [TileServer GL](https://github.com/mapbox/tileserver-gl) | Vector/raster tile server with GL styles |
| [Tegola](https://tegola.io/) | Go-based vector tile server for PostGIS |
| [GeoServer](https://geoserver.org/) | OGC-compliant WMS/WFS/WCS server (Java) |
| [MapProxy](https://mapproxy.org/) | WMS/WMTS proxy and tile cache |
| [Martin + Deck.gl](https://github.com/maplibre/martin) | PostGIS to deck.gl GeoJSON layer |

### Spatial Indexing Structures

| Structure | Description | Used By |
|-----------|-------------|---------|
| [R-tree](https://en.wikipedia.org/wiki/R-tree) | Bounding-box hierarchy for spatial objects | PostGIS (GiST), SQLite (R-tree), libspatialindex |
| [Quad-tree](https://en.wikipedia.org/wiki/Quadtree) | Recursive 2D space subdivision | MongoDB (2d index), Elasticsearch |
| [GeoHash](https://en.wikipedia.org/wiki/Geohash) | Hierarchical geocoding grid | Redis, MongoDB, DynamoDB, Elasticsearch |
| [S2 Geometry](https://s2geometry.io/) | Spherical geometry cells (Google) | Uber H3 (inspired by), MongoDB |
| [H3](https://h3geo.org/) | Hexagonal hierarchical index (Uber) | PostGIS (h3 extension), MongoDB, Redis |
| [GeoMesa](https://www.geomesa.org/) | Distributed spatio-temporal index | Accumulo, HBase, Cassandra, Kafka |
| [KD-tree](https://en.wikipedia.org/wiki/K-d_tree) | k-dimensional binary space partitioning | SciPy, PostGIS (KNN), Turf.js (nearest) |
| [Voronoi](https://en.wikipedia.org/wiki/Voronoi_diagram) | Proximity-based tessellation | Turf.js, PostGIS (ST_Voronoi), Shapely |
| [Delaunay](https://en.wikipedia.org/wiki/Delaunay_triangulation) | Dual of Voronoi (triangulation) | PostGIS, CGAL, Turf.js |

### PostGIS Spatial Functions (Key Operations)

| Category | Functions |
|----------|-----------|
| **Measurement** | `ST_Area`, `ST_Length`, `ST_Perimeter`, `ST_Distance`, `ST_DistanceSphere`, `ST_Azimuth` |
| **Relationships** | `ST_Contains`, `ST_Within`, `ST_Intersects`, `ST_Crosses`, `ST_Touches`, `ST_Disjoint`, `ST_Overlaps`, `ST_Equals` |
| **Set Operations** | `ST_Union`, `ST_Intersection`, `ST_Difference`, `ST_SymDifference`, `ST_Collect` |
| **Overlay** | `ST_Buffer`, `ST_Simplify`, `ST_SimplifyPreserveTopology`, `ST_MakeValid`, `ST_MakeLine` |
| **Centroids** | `ST_Centroid`, `ST_ClosestPoint`, `ST_PointOnSurface`, `ST_LineInterpolatePoint` |
| **Conversion** | `ST_GeomFromText`, `ST_GeomFromGeoJSON`, `ST_AsGeoJSON`, `ST_AsKML`, `ST_AsMVT` |
| **Transformation** | `ST_Transform`, `ST_SetSRID`, `ST_Centroid`, `ST_LineLocatePoint` |
| **Indexing** | `ST_MakeEnvelope`, `ST_Expand`, `&&` (bounding box overlap operator), GiST/BRIN indexes |
| **Clustering** | `ST_ClusterDBSCAN`, `ST_ClusterKMeans`, `ST_ClusterIntersecting` |
| **Raster** | `ST_AsRaster`, `ST_AsRaster`, `ST_MapAlgebra`, `ST_ZonalStats`, `ST_ExtractValues` |
| **TIN/Topology** | `ST_DelaunayTriangles`, `ST_VoronoiDiagrams`, `ST_BuildArea`, `ST_MakeValid` |
| **Geohash** | `ST_GeoHash`, `ST_Geohash`, `ST_PointFromGeoHash` |
| **Tile Generation** | `ST_AsMVT`, `ST_AsMVTGeom`, `ST_TileEnvelope`, `ST_AsPNG` |

### Django GIS (GeoDjango)

| Feature | Description |
|---------|-------------|
| [GeoDjango](https://docs.djangoproject.com/en/stable/ref/contrib/gis/) | ORM-level spatial field types, spatial queries, geometry operations |
| **Field Types** | `PointField`, `LineStringField`, `PolygonField`, `MultiPointField`, `MultiLineStringField`, `MultiPolygonField`, `GeometryCollectionField` |
| **Query Lookups** | `contains`, `within`, `intersects`, `touches`, `overlaps`, `crosses`, `disjoint`, `equals`, `same_as` |
| **Distance Queries** | `distance_lte`, `distance_gte`, `dwithin` (within distance) |
| **Spatial Functions** | `Area`, `Distance`, `Length`, `Perimeter`, `Centroid`, `Intersection`, `Difference`, `Union`, `Buffer`, `Simplify`, `Transform`, `MakeValid` |
| **Backends** | PostGIS, SpatiaLite, MySQL, Oracle |
| **Feeds** | `GeoRSS`, `GeoJSON`, `KML`, `KMZ` serialization |

### MySQL Spatial Functions (Key Operations)

| Category | Functions |
|----------|-----------|
| **Measurement** | `ST_Area`, `ST_Length`, `ST_Perimeter`, `ST_Distance`, `ST_Distance_Sphere` |
| **Relationships** | `ST_Contains`, `ST_Within`, `ST_Intersects`, `ST_Crosses`, `ST_Touches`, `ST_Disjoint` |
| **Set Operations** | `ST_Union`, `ST_Intersection`, `ST_Difference`, `ST_SymDifference` |
| **Overlay** | `ST_Buffer`, `ST_Envelope`, `ST_Centroid`, `ST_MakeValid` |
| **Conversion** | `ST_GeomFromText`, `ST_GeomFromGeoJSON`, `ST_AsGeoJSON`, `ST_AsKML`, `ST_GeomFromWKB` |
| **Indexing** | `SPATIAL INDEX` (R-tree), bounding box `MBR` operators |

### MongoDB Geospatial Queries

| Operation | Description |
|-----------|-------------|
| `$geoWithin` | Find documents within a geometry |
| `$geoIntersects` | Find documents that intersect a geometry |
| `$near` | Find nearest documents to a point |
| `$nearSphere` | Find nearest documents (spherical) |
| `geoNear` (aggregation) | Aggregation pipeline geospatial stage |
| `$box`, `$polygon`, `$center`, `$centerSphere` | Shape query operators |
| `2dsphere` index | Index for GeoJSON and coordinate pairs |

### Elasticsearch Spatial Queries

| Query Type | Description |
|------------|-------------|
| `geo_shape` query | Contains, within, intersects, disjoint |
| `geo_bounding_box` query | Documents within bounding box |
| `geo_distance` query | Documents within distance of point |
| `geo_polygon` query | Documents within polygon |
| `geo_sort` | Sort by distance from point |
| `geo_shape` aggregation | Spatial bucket aggregation |

### Redis Geospatial Commands

| Command | Description |
|---------|-------------|
| `GEOADD` | Add geospatial items |
| `GEODIST` | Distance between two members |
| `GEOSEARCH` | Search by radius or box |
| `GEORADIUS` | Members within radius (deprecated, use GEOSEARCH) |
| `GEOHASH` | Geohash strings for members |
| `GEOPOS` | Coordinates of members |
| `GEOSEARCHSTORE` | Store search results |

### Tile Pipeline Tools

| Tool | Role | Description |
|------|------|-------------|
| [Tippecanoe](https://github.com/felt/tippecanoe) | GeoJSON → MBTiles | Build vector tiles from GeoJSON with features like clustering, simplification, line joining |
| [ogr2ogr](https://gdal.org/programs/ogr2ogr.html) | Format converter | Convert between GeoJSON, Shapefile, KML, GML, CSV, PostGIS, etc. |
| [PMTiles](https://protomaps.com/) | Tile archive | Single-file cloud-native tile archives |
| [Martin](https://github.com/maplibre/martin) | Tile server | PostGIS/MBTiles/PMTiles → vector tiles |
| [TileServer GL](https://github.com/mapbox/tileserver-gl) | Tile server | Vector/raster tile serving with GL styles |
| [t-rex](https://t-rex.tileviewer.ch/) | Tile server | PostGIS → vector tiles (Rust) |
| [Tiles.xyz](https://github.com/mapbox/tiles.xyz) | Tile hosting | Cloud-native tile hosting |

### Geocoding & Routing Backends

| Tool | Type | Description |
|------|------|-------------|
| [Nominatim](https://nominatim.org/) | Geocoder | OpenStreetMap forward/reverse geocoder |
| [Pelias](https://github.com/pelias/pelias) | Geocoder | Open-source geocoder (geocode.earth) |
| [Photon](https://photon.komoot.io/) | Geocoder | Open-source geocoder (based on OSM) |
| [LibreTranslate](https://libretranslate.com/) | Translator | Self-hosted translation API |
| [OSRM](http://project-osrm.org/) | Router | Open-source routing machine (C++) |
| [Valhalla](https://valhalla.github.io/) | Router | Open-source routing engine (C++) |
| [GraphHopper](https://www.graphhopper.com/) | Router | Open-source routing engine (Java) |
| [OpenTripPlanner](https://www.opentripplanner.org/) | Multimodal router | Public transit + car + bike + walk routing |
| [OpenRouteService](https://openrouteservice.org/) | Router | Multimodal routing with profiles |
| [RouterOSM](https://github.com/Router-OSM) | Router | OSM-based routing service |

### Core GIS Libraries (Language-Agnostic)

These are foundational C/C++ libraries used across multiple language bindings:

- Library - Description
- [GDAL](https://gdal.org/) - Raster + vector geospatial data abstraction
- [OGR](https://gdal.org/ogr/) - Vector data processing (part of GDAL)
- [GEOS](https://libgeos.org/) - Geometry engine (JTS C++ port)
- [PROJ](https://proj.org/) - Coordinate reference system transformations
- [PostGIS](https://postgis.net/) - Spatial SQL extension for PostgreSQL
- [SpatiaLite](https://www.gaia-gis.it/gaia-sins/) - Spatial SQL extension for SQLite
- [libspatialindex](https://github.com/libspatialindex/libspatialindex) - R-tree spatial indexing
- [Mapnik](https://mapnik.org/) - Server-side 2D map rendering
- [H3](https://h3geo.org/) - Uber's hexagonal hierarchical spatial index

---

---

