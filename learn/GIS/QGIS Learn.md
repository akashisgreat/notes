## 1. What is GIS?
**GIS (Geographic Information System)** is a system for storing, analyzing, and visualizing information that has a location on Earth.

Example:
- Civil engineer → Road planning
- Surveyor → Land boundaries
- Government → Population maps
- Farmer → Soil maps

## 2. Raster vs Vector Data
### Vector Data
- **Point** → One location
- **Line** → A path
- **Polygon** → An area
### Raster Data
- Satellite images
- Drone photos
- Google Earth imagery
- Elevation (DEM)
- Rainfall maps
## 3. Coordinates (Latitude and Longitude)

Every location on Earth can be described with two numbers.
e.g. : Lat, Long; easting, northing etc.

## 4. Coordinate Reference System (CRS)
Coordinates need a reference system so everyone interprets them the same way.

A **CRS** defines:
- The shape/model of the Earth used.
- The units (degrees or meters).
- How coordinates are projected onto a flat map.

Examples:
- **EPSG:4326 (WGS84)** → Latitude/Longitude in degrees (used by GPS).
- **EPSG:3857 (Web Mercator)** → Used by many online maps.
- **UTM** → Coordinates in meters, useful for measuring distances and areas.
-  and many more.

Without the correct CRS, layers may appear in the wrong place.

## 5. Layers
has multiple layers for multiple objects.
- layers for road
- layers for rivers
- layers for villages
- layers for satellites images
- layers for custom draw digitize (line, points)

## 6. Attributes

e.g.:

| Village  | Population | Area (km²) |
| -------- | ---------- | ---------- |
| Rampur   | 2,500      | 4.5        |
| Mohanpur | 1,800      | 3.2        |
The _map_ shows **where the feature** is, while the _attribute table stores_ **information about** the feature.

## 7. Common GIS File Formats

### Shapefile (.shp)

One of the oldest and most widely supported vector formats.

A shapefile actually consists of several files that work together:

```
road.shp
road.dbf
road.shx
road.prj
```

---

### GeoJSON (.geojson)

A modern text-based format.

Advantages:

- Easy to read.
- Used by web maps.
- Simple to share.

---

### KML (.kml)

Developed for Google Earth.

Used for:

- GPS tracks.
- Google Earth.
- Google Maps imports.

---

### GeoTIFF (.tif)

A raster image that also stores geographic location information.

Used for:

- Satellite imagery.
- DEMs.
- Aerial photography.

---

# How these concepts fit together

```
                GIS
                 │
        ┌────────┴────────┐
        │                 │
    Vector Data      Raster Data
        │                 │
   Points/Lines/      Pixels
     Polygons
        │                 │
        └──────┬──────────┘
               │
          Coordinates
               │
              CRS
               │
          Display in QGIS
               │
        Layers + Attributes
               │
          Create Maps
```

These are the core ideas you'll use throughout QGIS. Once you're comfortable with them, opening QGIS and working with real data becomes much easier.