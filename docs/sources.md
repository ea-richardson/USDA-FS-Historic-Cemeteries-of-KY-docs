---
title: Sources
nav_order: 2
---

# Sources
{: .no_toc }

While the reliability and completeness of these sources vary, they collectively provide a strong foundation for initial cemetery mapping efforts.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Hierarchy

For the current phase, the general order of authority is:

1. 1950s USGS topos
2. USFS cemetery datasets (2)
3. County and state historical society records
4. Modern imagery, mapping sources, and online cemetery databases, including Find a Grave

This hierarchy does not imply that lower-ranked sources are inherently less trustworthy. Rather, it establishes which sources should generally guide the initial GIS representation when evidence conflicts. Lower-ranked sources may still provide important information that is absent from higher-ranked sources.

{: .note }
Only USGS topos and two USFS datasets are currently being used to create cemetery geometry. All other sources are being used to identify and attribute cemeteries.

## Primary
- **1950s USGS 7.5-minute quadrangles (1:24,000-scale topographic map)**
    - Available for download from:
        - Esri [Historical Topo Map Explorer](https://livingatlas.arcgis.com/topomapexplorer)
        - USGS [topoView](https://ngmdb.usgs.gov/topoview/viewer) (*Downloads require extraction from a zipped folder before use.*)
- **USFS Daniel Boone National Forest (2013 Cemetery Points)**
    - Used to digitize cemeteries not appearing on topographic maps
    - Point feature class: *KY_CemData_Draft2013*
- **USFS Archaeological Cemetery Center Points**
    - Also used to digitize cemeteries absent from topographic maps
    - Point feature class: *Archaeology_HistoricCem_Centerpoints*

## Supporting
These sources are useful for identification, interpretation, attribution, and cross-checking, but **should not automatically override the primary evidence.**

At this stage, the crowd-sourced Find a Grave site alone is not sufficient to establish a cemetery for inclusion unless the cemetery is also found in one or more of the project's primary sources.

- **[Find a Grave](https://www.findagrave.com/cemetery-browse/USA/Kentucky?id=state_19)**
    - Used to identify cemeteries and assign attributes (e.g., "Burial_Number" and "Link")
    - Point feature class: *KYCemPts_FindaGrave_01292025* (courtesy of Ancestry)
- **Zip Code Zones**
    - Available from Esri's [Living Atlas](https://www.arcgis.com/home/item.html?id=5f31109b46d541da86119bd4cf213848) (*updated annually*)
    - Polygon feature class: *zip_zones* (*created using the Living Atlas layer in September 2024*)
- **State and County Boundaries**
    - Sourced from the [US Census Bureau](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html)
- **[Google Maps & Google Street View](https://maps.google.com/)**
- **[Kentucky Natural Color Imagery (2024)](https://kygeonet.maps.arcgis.com/home/webmap/viewer.html)**

## Additional
- **[OpenStreetMap](https://www.openstreetmap.org/)**  
- **[Regrid Property App Parcel Data](https://app.regrid.com/us/ky)** (*Used for determining cemetery land ownership and boundaries*)  
- **[Cemeteries in Kentucky Database](https://kyhistory.com/digital/collection/LIB/id/493/)** (*Kentucky Historical Society Library Collection*)  
- **County Historical Societies & Genealogical Associations** (*Local burial records and cemetery maps*)  
- **Local History Books & Manuscripts** (*County histories and cemetery transcription projects*)

### Online Cemetery Databases
These sources contain cemetery data that may be outdated, incomplete, or inconsistent. Users should cross-reference multiple sources and document concerns in the "Comments" field. When possible, information should be verified using historical maps, property records, or local historical societies.

- **[Billion Graves](https://billiongraves.com/search/cemetery)**
- **[Interment.net](https://www.interment.net/us/ky/index.htm)** 
- **[The Tombstone Transcription Project](http://www.usgwtombstones.org/kentucky/kentucky.html)**
- **[Kentucky Genealogy Cemetery Index](https://kentuckygenealogy.org/cemeteries)**
- **[USGenWeb Archives for Kentucky](http://files.usgwarchives.net/ky/)**
- **[Central Kentucky Cemetery Maps (Lexington Public Library)](https://www.lexpublib.org/cemeteries)**
- **[The Ancestor Hunt – Free Kentucky Cemetery Records](https://theancestorhunt.com/blog/free-kentucky-online-cemetery-and-burial-records/)** 
- **[Churches and Cemeteries in Kentucky](https://churches-and-cemeteries.com/index_pages/KY.html)**
- **[WikiTree Cemetery Category](https://www.wikitree.com/wiki/Category:Kentucky%2C_Cemeteries)**
- **[USGenWeb Archives](http://usgwarchives.net/ky/kyfiles.html)**
