---
title: Digitization
nav_order: 4
---

# Digitization
{: .no_toc }

While the reliability and completeness of these sources vary, they collectively provide a strong foundation for initial cemetery mapping efforts.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Manually digitize cemetery polygons using the topographic maps. (The point feature class will be created later.) Review topographic map symbols with this [map key](https://www.historicaerials.com/topo-map-key) or consult the [USGS documentation](https://pubs.usgs.gov/gip/TopographicMapSymbols/topomapsymbols.pdf).

Features in the *Archaeology_HistoricCem_Centerpoints* and *KY_CemData_Draft2013* feature classes should also be digitized.

* If a feature is near or overlaps a cemetery digitized from a topographic map, include a note in the "Comments" field. For example:
    * _Also included in archaeological data from USFS Daniel Boone National Forest._
    * _Also included in 2013 cemetery data from USFS Daniel Boone National Forest._
* If a feature is not near or overlapping a cemetery, approximate the cemetery size and create a polygon with the point in the center. Add a note in the "Comments" field. For example:
    * _Digitized using archaeological cemetery center point data from USFS Daniel Boone National Forest._
    * _Digitized using 2013 cemetery point data from USFS Daniel Boone National Forest._

1. **After digitizing all cemeteries for a quadrangle, populate the *Topo_Name* field.**
2. **Before proceeding to attribute data entry, save save your edits and save the project to avoid losing any work.**

## Standards
### Geometry
When a cemetery boundary is shown:
1. Digitize the cemetery polygon based on the historical map representation.
2. The polygon should follow the mapped cemetery boundary as closely as practical.
3. Do not enlarge or shrink the cemetery simply because a larger area appears plausible or is reflected in modern imagery.

When a cemetery symbol or label is present but no boundary is shown, OR when collecting cemeteries present in USFS point datasets but not present on the topo:
1. Create a 0.25-acre default polygon.
2. Center the polygon on the mapped location.
3. Document in `Comments` that the polygon represents the default 0.25-acre approximation because no mapped boundary was available.

{: .note }
The default size is a working project convention, not a claim that the cemetery actually occupied exactly 0.25 acres.

### Names
The current working schema contains two cemetery name fields. The `Comments` field may also be used to include additional names or naming variants. **Populate these fields in order of source hierarchy and do NOT discard alternate names.**

For example, if sources provide:
* Smith Cemetery (Source: USGS topo)
* Smith Family Cemetery (Source: Find a Grave)
* Smith Burial Ground (Source: Find a Grave)
* Old Smith Cemetery (Source: County Historical Society Data)

then the primary name fields should contain:
* **Cemetery_Name1**: Smith Cemetery
* **Cemetery_Name2**: Old Smith Cemetery

The remaining names can be documented in `Comments`: "Also known as Smith Family Cemetery and Smith Burial Ground."

The exact formatting of comments may be standardized later.
