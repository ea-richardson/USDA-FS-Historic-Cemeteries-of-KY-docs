---
title: Metadata
nav_order: 6
---

# Metadata
{: .no_toc }

Proper metadata ensures the data is well-documented, easily understood, and can be efficiently shared and maintained. Follow the instructions below to complete the metadata entry process.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Run the cell below to ensure your name appears in the credits
```python
name = input("Please enter your full name (as it will appear in the credits): ")
```

## Create a cemetery point feature class

```python
arcpy.management.FeatureToPoint(
    in_features=f"{county}_County",
    out_feature_class=f"{gdb_path}\{county}_County_Points",
    point_location="CENTROID"
)
```

If you have encounter an issue running the code below, check if the metadata is read-only:
- Right-click the feature class in the **Contents** pane.
- Select **Properties**.
- Go to the **Metadata** tab.
- Click the dropdown and select "Layer has its own metadata".
- Save by selecting **Apply**/**OK**.
- You may also need to restart ArcGIS Pro.

## Create point feature class metadata

*How It Works:*
- *The script uses a regex pattern to extract the state abbreviation, place name, and year from GeoTIFF filenames and adds these details to a list.*
- *It searches for a feature class layer ({county}_County_Points) in the active map and checks if the metadata can be updated.*
- *It updates the metadata for the feature class with new title, summary, description, tags, and credits, using the extracted GeoTIFF details and predefined metadata fields.*

```python
import os
import re
import arcpy
from arcpy import metadata as md

# Define the topo_list
topo_list = []

# Regex pattern to match the filenames
pattern = r'([A-Za-z]{2})_([^_]+)_\d{6}_(\d{4})_\d+_geo\.tif'

# Loop through all files in the specified directory
for filename in os.listdir(county_directory):
    # Check if the file is a GeoTIFF (ends with .tif)
    if filename.lower().endswith('.tif'):
        # Use regex to match the filename and extract details
        match = re.match(pattern, filename)
        if match:
            state_abbr = match.group(1)  # State abbreviation (KY, IN, etc.)
            place_name = match.group(2)  # Place name (Claysville, Mount Olivet, etc.)
            year = match.group(3)  # Year (e.g., 1952)
            
            # Construct the topo list entry
            topo_list.append(f"{state_abbr} {place_name} {year}")
        else:
            print(f"Filename does not match the expected pattern: {filename}")

# If no topographic files are found
if not topo_list:
    print("No topographic maps found in the specified directory.")
else:
    print(f"Found {len(topo_list)} topographic map(s).")

# Convert topo_list to a comma-separated string
topo_list_str = ', '.join(topo_list)

# Get the active ArcGIS project
aprx = arcpy.mp.ArcGISProject("CURRENT")
current_map = aprx.activeMap

# Find the feature class layer with name matching "{county}_County_Points"
feature_layer = None
for layer in current_map.listLayers():
    if layer.isFeatureLayer and layer.name == f"{county}_County_Points":
        feature_layer = layer
        break

# Check if a matching feature layer was found
if feature_layer:
    # Get the Metadata object for the feature class
    feature_md = md.Metadata(feature_layer)
    
    # Construct the new title
    new_title = f"{county} County Historical Cemeteries"
    
    # Set the new tags
    new_tags = f"cemetery, Kentucky, {county} County, historical, grave"

    # Set the new summary
    new_summary = (f"This point feature class serves as a record of historical cemeteries "
                   f"within {county} County, Kentucky, as digitized from historical "
                   f"USGS georeferenced topographic maps, Find A Grave, 2013 cemetery point data from USFS "
                   f"Daniel Boone National Forest, and archaeological cemetery center point data from USFS "
                   f"Daniel Boone National Forest.")

    # Set the new description
    new_description = (f"Cemetery polygons are digitized by overlying historic USGS georeferenced topographic maps "
                       f"and drawing polygons around cemetery locations on the historic topographic maps. Point data "
                       f"provided by USFS Daniel Boone National Forest is also used as reference points for cemetery "
                       f"locations, with polygons digitized over the point locations with the points at the center. "
                       f"The schema for each feature class is filled in with available information from county "
                       f"historical societies, the Find A Grave website, and other internet research.\n\n"
                       f"USGS georeferenced topographic maps used (1:24,000 scale):\n\n{topo_list_str}.")

    # Set the fixed credits
    fixed_credits = f"United States Forest Service Daniel Boone National Forest, Kentucky Heritage Council, State of Kentucky Commonwealth Office of Technology GIS, {name}"

    # Update the metadata
    if not feature_md.isReadOnly:
        feature_md.title = new_title
        feature_md.summary = new_summary  # Set the summary to the new format
        feature_md.description = new_description  # Set the description
        feature_md.tags = new_tags  # Set tags to the predefined list
        feature_md.credits = fixed_credits  # Fixed credits
        
        # Save changes
        feature_md.save()
        print("Metadata updated successfully!")
    else:
        print("Metadata is read-only and cannot be updated.")
else:
    print(f"No feature class found with the name '{county}_County_Points'.")
```

## Create polygon feature class metadata

```python
import os
import re
import arcpy
from arcpy import metadata as md

# Define the topo_list
topo_list = []

# Regex pattern to match the filenames
pattern = r'([A-Za-z]{2})_([^_]+)_\d{6}_(\d{4})_\d+_geo\.tif'

# Loop through all files in the specified directory
for filename in os.listdir(county_directory):
    # Check if the file is a GeoTIFF (ends with .tif)
    if filename.lower().endswith('.tif'):
        # Use regex to match the filename and extract details
        match = re.match(pattern, filename)
        if match:
            state_abbr = match.group(1)  # State abbreviation (KY, IN, etc.)
            place_name = match.group(2)  # Place name (Claysville, Mount Olivet, etc.)
            year = match.group(3)  # Year (e.g., 1952)
            
            # Construct the topo list entry
            topo_list.append(f"{state_abbr} {place_name} {year}")
        else:
            print(f"Filename does not match the expected pattern: {filename}")

# If no topographic files are found
if not topo_list:
    print("No topographic maps found in the specified directory.")
else:
    print(f"Found {len(topo_list)} topographic map(s).")

# Convert topo_list to a comma-separated string
topo_list_str = ', '.join(topo_list)

# Get the active ArcGIS project
aprx = arcpy.mp.ArcGISProject("CURRENT")
current_map = aprx.activeMap

# Find the feature class layer with name matching "{county}_County"
feature_layer = None
for layer in current_map.listLayers():
    if layer.isFeatureLayer and layer.name == f"{county}_County":
        feature_layer = layer
        break

# Check if a matching feature layer was found
if feature_layer:
    # Get the Metadata object for the feature class
    feature_md = md.Metadata(feature_layer)
    
    # Construct the new title
    new_title = f"{county} County Historical Cemeteries"
    
    # Set the new tags
    new_tags = f"cemetery, Kentucky, {county} County, historical, grave"

    # Set the new summary
    new_summary = (f"This polygon feature class serves as a record of historical cemeteries "
                   f"within {county} County, Kentucky, as digitized from historical "
                   f"USGS georeferenced topographic maps, Find A Grave, 2013 cemetery point data from USFS "
                   f"Daniel Boone National Forest, and archaeological cemetery center point data from USFS "
                   f"Daniel Boone National Forest.")

    # Set the new description
    new_description = (f"Cemetery polygons are digitized by overlying historic USGS georeferenced topographic maps "
                       f"and drawing polygons around cemetery locations on the historic topographic maps. Point data "
                       f"provided by USFS Daniel Boone National Forest is also used as reference points for cemetery "
                       f"locations, with polygons digitized over the point locations with the points at the center. "
                       f"The schema for each feature class is filled in with available information from county "
                       f"historical societies, the Find A Grave website, and other internet research.\n\n"
                       f"USGS georeferenced topographic maps used (1:24,000 scale):\n\n{topo_list_str}.")

    # Set the fixed credits
    fixed_credits = f"United States Forest Service Daniel Boone National Forest, Kentucky Heritage Council, State of Kentucky Commonwealth Office of Technology GIS, {name}"

    # Update the metadata
    if not feature_md.isReadOnly:
        feature_md.title = new_title
        feature_md.summary = new_summary  # Set the summary to the new format
        feature_md.description = new_description  # Set the description
        feature_md.tags = new_tags  # Set tags to the predefined list
        feature_md.credits = fixed_credits  # Fixed credits
        
        # Save changes
        feature_md.save()
        print("Metadata updated successfully!")
    else:
        print("Metadata is read-only and cannot be updated.")
else:
    print(f"No feature class found with the name '{county}_County'.")
```
