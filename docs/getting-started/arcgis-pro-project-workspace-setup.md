---
title: ArcGIS Pro Project Workspace Setup
parent: Getting Started
nav_order: 4
---

# ArcGIS Pro Project Workspace Setup
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Add *HistoricalCemeteries_KY* Geodatabase
**Select and add the *HistoricalCemeteries_KY* geodatabase to the project:**

*How It Works:*
- *The user selects a geodatabase folder via a file dialog using Tkinter.*
- *The script checks if the selected folder is a valid geodatabase and if the project is not read-only.*
- *The selected geodatabase is added to the project, the databases list is updated, and the project is saved.*

```python
import arcpy
import os
import tkinter as tk
from tkinter import filedialog

# Initialize Tkinter window (for file dialog)
root = tk.Tk()
root.withdraw()  # Hides the root window

# Prompt user to select a geodatabase
gdb_path = filedialog.askdirectory(title="Select HistoricalCemeteries_KY.gdb")

# Ensure the path is a valid geodatabase
if gdb_path and os.path.isdir(gdb_path) and gdb_path.lower().endswith('.gdb'):
    # Get the active ArcGIS Pro project
    aprx = arcpy.mp.ArcGISProject("CURRENT")  # Refers to the active project
    
    # Check if the project is read-only
    if aprx.isReadOnly:
        print('WARNING: Project is read-only. Exiting.')
        exit()
    
    # Get the current list of databases in the project
    db = aprx.databases
    
    # Append the selected geodatabase to the project (not as default)
    db.append({'databasePath': gdb_path, 'isDefaultDatabase': False})
    
    # Update the project's databases list
    aprx.updateDatabases(db, True)
    
    # Save the project after making the changes
    aprx.save()
    
    print(f"Geodatabase '{gdb_path}' added to the project.")
else:
    print("Invalid selection or no geodatabase selected. Exiting.")
```

***Make *HistoricalCemeteries_KY.gdb* the default database:***

```python
import arcpy

# Get the active ArcGIS project
aprx = arcpy.mp.ArcGISProject("CURRENT")

# Set the default geodatabase
aprx.defaultGeodatabase = gdb_path

# Save the project to apply the change
aprx.save()

# Print confirmation
print(f"Default geodatabase set to: {gdb_path}")
```

## Create and/or Rename a Map
**Rename an existing map or create a new map:**

*How It Works:*
- *The script lists all existing maps in the current ArcGIS Pro project, allowing the user to choose either to rename an existing map or create a new one.*
- *If creating a new map, the script checks for any naming conflicts and prompts the user to provide a new name if needed.*
- *The selected map is renamed based on a predefined county variable.*

```python
import arcpy

# Function to list all maps
def list_all_maps():
    try:
        aprx = arcpy.mp.ArcGISProject("CURRENT")
        maps = aprx.listMaps()
        return maps
    except Exception as e:
        arcpy.AddMessage(f"Error: {str(e)}")
        return []

# Function to rename an existing map
def rename_map(map_obj, new_name):
    try:
        map_obj.name = new_name
        arcpy.AddMessage(f"Map renamed to {new_name}")
    except Exception as e:
        arcpy.AddMessage(f"Error renaming map: {str(e)}")

# Function to create a new map
def create_new_map(aprx):
    # Check if a map with the same name exists
    maps = aprx.listMaps()
    new_map_name = f"{county}_County"
    
    # Check for duplicate names and prompt user if necessary
    existing_map_names = [map_obj.name for map_obj in maps]
    if new_map_name in existing_map_names:
        user_choice = input(f"A map with the name '{new_map_name}' already exists. Would you like to rename the new map? (y/n): ")
        if user_choice.lower() == "y":
            new_map_name = input("Enter a new name for the new map: ")
        else:
            arcpy.AddMessage("New map creation canceled.")
            return None
    
    try:
        new_map = aprx.createMap("New Map")
        # Rename the new map to the determined name
        rename_map(new_map, new_map_name)
        arcpy.AddMessage(f"New map created and renamed: {new_map.name}")
        return new_map
    except Exception as e:
        arcpy.AddMessage(f"Error creating new map: {str(e)}")
        return None

# Function to handle the renaming or creation process
def rename_or_create_map():
    aprx = arcpy.mp.ArcGISProject("CURRENT")
    maps = list_all_maps()

    # List all existing maps
    if len(maps) > 0:
        print("The following maps exist:")
        for idx, map_obj in enumerate(maps):
            print(f"{idx + 1}. {map_obj.name}")
    
    # Always ask the user if they want to create a new map
    user_choice = input(f"Which map would you like to rename (1-{len(maps)})? Or, enter '0' to create a new map: ")

    if user_choice == '0':
        # Create a new map and rename it to county_County
        return create_new_map(aprx), True
    else:
        try:
            selected_map = maps[int(user_choice) - 1]
            rename_map(selected_map, f"{county}_County")
            return selected_map, True
        except (ValueError, IndexError):
            arcpy.AddMessage("Invalid input, no map selected.")
            return None, False

# Main execution logic
maps = list_all_maps()

if len(maps) > 0:
    # If maps exist, list them
    for map_obj in maps:
        arcpy.AddMessage(f"Existing map: {map_obj.name}")
    result_map, action_taken = rename_or_create_map()

    if action_taken:
        summary_message = f"Map '{result_map.name}' has been renamed to {county}_County."
    else:
        summary_message = "No action taken."
else:
    # No maps exist, inform the user
    result_map, action_taken = rename_or_create_map()
    if action_taken:
        summary_message = f"Map has been renamed to {county}_County."
    else:
        summary_message = "No map created."

# Print summary message
print(summary_message)
arcpy.AddMessage(summary_message)
```

## Add GeoTIFFs
**Add GeoTIFF files to the active map:**

*How It Works:*
- *The script scans the county directory for valid GeoTIFF files, excluding those with "TMorth" in the filename.*
- *It matches the filenames with a list of quadrangle names and adds the corresponding GeoTIFF files to the active map.*

*The order of the quads in the list determines the order in which the GeoTIFFs are added to the map. The last item in the list is added first, with the topmost GeoTIFF covering the northwest corner of the county and the bottommost covering the southeast corner.*

```python
import arcpy
import os
import time  # Import time module to add a delay

def add_geotiffs_to_map(county_dir, quads, map_obj, delay_seconds=1):
    # Ensure the county directory exists
    if not os.path.isdir(county_dir):
        print(f"The directory {county_dir} does not exist.")
        return

    # Step 1: Get all GeoTIFF files within the county directory
    valid_geotiffs = []

    # Walk through the county directory to find all .tif files excluding those with "TMorth" in the filename
    for dirpath, dirnames, filenames in os.walk(county_dir):
        for filename in filenames:
            if filename.lower().endswith('.tif') and "tmorth" not in filename.lower():
                valid_geotiffs.append(os.path.join(dirpath, filename))

    # Step 2: Provide feedback on the number of GeoTIFFs found
    if len(valid_geotiffs) == 0:
        print("No valid GeoTIFFs found in the directory.")
    else:
        print(f"Found {len(valid_geotiffs)} valid GeoTIFF(s). Adding to the map...")

        added_geotiffs = set()
        for quad in quads:
            # Normalize the quad name by removing spaces and converting to lowercase
            quad_normalized = quad.replace(" ", "").lower()

            # Find matching GeoTIFFs
            matching_geotiffs = [geotiff for geotiff in valid_geotiffs 
                                 if quad_normalized in os.path.basename(geotiff).replace(" ", "").lower()]

            # Add the matching GeoTIFF to the map (if it's not already added)
            for geotiff in matching_geotiffs:
                if geotiff not in added_geotiffs:
                    try:
                        # Normalize path to always use forward slashes
                        geotiff = geotiff.replace("\\", "/")  # Convert backslashes to forward slashes
                        
                        # Handle spaces in GeoTIFF filenames by ensuring they're properly processed
                        if " " in os.path.basename(geotiff):
                            print(f"GeoTIFF with spaces detected: {geotiff}")
                        
                        map_obj.addDataFromPath(geotiff)
                        added_geotiffs.add(geotiff)  # Track as added
                        print(f"Added {geotiff} to the map.")
                        
                        # Introduce a time lag before adding the next GeoTIFF
                        time.sleep(delay_seconds)  # Delay in seconds

                    except Exception as e:
                        print(f"Error adding {geotiff} to the map: {e}")

        print(f"{len(added_geotiffs)} GeoTIFFs added to the map.")

# Assuming the map object is already available in ArcGIS Pro
# Get the current map from ArcGIS Pro
aprx = arcpy.mp.ArcGISProject("CURRENT")
map_obj = aprx.activeMap  # Get the active map in the project

# Check if the directory exists and add GeoTIFFs to the map
if os.path.isdir(county_directory):
    add_geotiffs_to_map(county_directory, quads, map_obj, delay_seconds=1)  # Set delay to 2 seconds between each addition
else:
    print(f"The county directory {county_directory} does not exist.")
```

**Group GeoTIFFs in the Contents pane:**

*How It Works:*
- *The script checks if a "topos" group already exists in the active map; if not, it creates one.*
- *It loops through all layers in the map, adding all .tif raster layers to the "topos" group in reverse order.*
- *It removes any .tif layers that are not part of the "topos" group, cleaning up the map.*

```python
import arcpy

# Get the active map from the currently open project
aprx = arcpy.mp.ArcGISProject("CURRENT")  # Refers to the active project
map_obj = aprx.activeMap  # Get the active map

# Check if the map is valid
if map_obj is None:
    print("No active map found.")
else:
    # Create a new group layer named "topos" if it doesn't already exist
    group_name = "topos"
    existing_groups = [layer for layer in map_obj.listLayers() if layer.isGroupLayer]

    # Check if the "topos" group already exists, if not, create it
    topos_group = None
    for group in existing_groups:
        if group.name == group_name:
            topos_group = group
            break

    if topos_group is None:
        # Create a new group layer called "topos"
        topos_group = map_obj.createGroupLayer(group_name)
    
    # Loop through layers and add all GeoTIFFs (.tif) to the "topos" group in reverse order
    for layer in map_obj.listLayers():  # Reverse the list of layers
        if layer.isRasterLayer and layer.name.lower().endswith(".tif"):  # Check for .tif layers
            # Move the GeoTIFF layer to the "topos" group
            map_obj.addLayerToGroup(topos_group, layer)
    
    # Remove all ungrouped .tif layers (those not in the "topos" group)
    for layer in map_obj.listLayers():
        if layer.isRasterLayer and layer.name.lower().endswith(".tif"):
            # Check if the layer is not part of the "topos" group
            if layer not in topos_group.listLayers():
                # Remove the ungrouped .tif layer from the map
                map_obj.removeLayer(layer)
    
    print(f"All .tif layers have been grouped under '{group_name}' group in reversed order, and ungrouped layers have been removed.")
```

## Add County Boundaries
**Add the *ky_counties* feature class and update symbology to create a boundary outline of the county of interest:**

*How It Works:*
- *The script checks if the specified feature class exists in the given geodatabase.*
- *It adds the feature class to the active map and customizes its symbology with a transparent fill, colored outline, and offset effect.*
- *It applies a definition query to filter the layer based on the given county name and saves the project.*

```python
import arcpy
import os

# Specify the feature class name you are looking for
fc_name = "ky_counties"

# Search for the feature class in the geodatabase
if gdb_path and os.path.isdir(gdb_path):
    # Construct the path to the feature class
    fc_path = os.path.join(gdb_path, fc_name)
    
    if arcpy.Exists(fc_path):
        print(f"Feature class '{fc_name}' found in {gdb_path}.")
        
        # Get the active project and map
        aprx = arcpy.mp.ArcGISProject("CURRENT")
        map_obj = aprx.activeMap

        # Add the feature class to the map
        new_layer = map_obj.addDataFromPath(fc_path)
        
        # Apply the symbology with a SimpleRenderer
        sym = new_layer.symbology
        sym.updateRenderer('SimpleRenderer')  # Use the SimpleRenderer
        
        # Apply the symbol settings
        symbol = sym.renderer.symbol
        
        # Set color to transparent fill
        symbol.color = {'RGB': [0, 0, 0, 0]}  # Transparent fill (alpha 0)
        
        # Set outline color (red = 169, green = 0, blue = 230)
        symbol.outlineColor = {'RGB': [169, 0, 230, 100]}  # Outline: RGB(169, 0, 230) with full opacity
        
        # Set outline width to 5 points
        symbol.outlineWidth = 5  # Outline width set to 5 points
        
        # Apply an outline offset effect (5 pts)
        symbol.outlineOffsetX = 5  # Horizontal offset
        symbol.outlineOffsetY = 5  # Vertical offset
        
        # Apply the updated symbology back to the layer
        new_layer.symbology = sym
        
        # Apply the definition query based on the "county" variable
        new_layer.definitionQuery = f"NAME = '{county}'"
        
        print(f"Definition query applied: NAME = '{county}'")
        
        # Save the project after changes
        aprx.save()
    else:
        print(f"Feature class '{fc_name}' does not exist in the specified geodatabase.")
else:
    print("Invalid geodatabase path. Please check and try again.")
```

## Create Cemetery Polygon Feature Class
**Create a copy of *Cemetery_PolygonFC_Template*, rename, and change symbology:**

*How It Works:*
- *The script copies the "Cemetery_PolygonFC_Template" feature class to a new feature class, named after the county.*
- *It checks if the new layer is already in the map; if not, it adds the new feature class to the map.*
- *It applies custom symbology to the new layer, setting it to a semi-transparent bright pink with a transparent outline, then saves the project.*

```python
import arcpy
import os

# Define the source feature class and the new name
source_fc = "Cemetery_PolygonFC_Template"
new_fc_name = f"{county}_County"

# Construct the full paths for source and destination feature classes
source_fc_path = os.path.join(gdb_path, source_fc)
new_fc_path = os.path.join(gdb_path, new_fc_name)

# Check if the source feature class exists
if arcpy.Exists(source_fc_path):
    # Copy the feature class to the new location with the new name
    arcpy.CopyFeatures_management(source_fc_path, new_fc_path)
    print(f"Polygon cemetery feature class created for {county} County: '{new_fc_name}'.")
else:
    print(f"Feature class '{source_fc}' does not exist in the specified geodatabase.")

# Get the map and check if the layer is already present before adding it
aprx = arcpy.mp.ArcGISProject("CURRENT")
map_obj = aprx.activeMap

# Check if the layer is already in the map, and if not, add it
layer_exists = False
for layer in map_obj.listLayers():
    if layer.name == new_fc_name:
        layer_exists = True
        break

if not layer_exists:
    new_layer = map_obj.addDataFromPath(new_fc_path)
    print(f"Added '{new_fc_name}' to the map.")
else:
    print(f"Layer '{new_fc_name}' is already in the map.")

# Access the symbology of the new layer
new_layer = map_obj.listLayers(new_fc_name)[0]
sym = new_layer.symbology

# Set the symbology to a SimpleRenderer
sym.updateRenderer("SimpleRenderer")

# Apply the semi-transparent bright pink color (Red 255, Green 0, Blue 197, 45% transparency)
sym.renderer.symbol.color = {'RGB': [255, 0, 197, 0.25]}  # 25% transparency

# Make the outline completely transparent
sym.renderer.symbol.outlineColor = {'RGB': [255, 255, 255, 0]}  # Transparent outline (alpha = 0)

# Apply the updated symbology to the layer
new_layer.symbology = sym

# Save the project after making changes
aprx.save()

print(f"Symbology applied: Semi-transparent bright pink fill with a transparent outline.")
```

## Add Cemetery Point Data
**Add *KYCemPts_FindaGrave_01292025*, *Archaeology_HistoricCem_Centerpoints*, and *KYCemPts_FindaGrave_01292025* feature classes to the map with custom symbology:**

*How It Works:*
- *The script checks if each feature class exists in the geodatabase and adds it to the active map.*
- *It applies specific symbology based on the feature class name, using symbols such as 3D pushpins and circle markers.*
- *After applying symbology to the layers, the script saves the ArcGIS project.*

```python
import arcpy
import os

# Define the feature classes
feature_classes = [
    "KY_CemData_Draft2013",
    "Archaeology_HistoricCem_Centerpoints",
    "KYCemPts_FindaGrave_01292025"
]

# Get the active ArcGIS project and map
aprx = arcpy.mp.ArcGISProject("CURRENT")  # Get the current ArcGIS Project
map_obj = aprx.activeMap  # Get the active map

# Function to apply symbology to point feature classes (for color and outline)
def apply_point_symbology(layer, fill_color, outline_color, outline_width, size):
    sym = layer.symbology
    sym.updateRenderer("SimpleRenderer")  # Apply the SimpleRenderer for point features
    
    # Set the fill color (using RGB)
    sym.renderer.symbol.color = {'RGB': fill_color}
    
    # Set the outline color (using RGB)
    sym.renderer.symbol.outlineColor = {'RGB': outline_color}
    
    # Set the outline width
    sym.renderer.symbol.outlineWidth = outline_width
    
    # Set the point size
    sym.renderer.symbol.size = size
    
    # Apply the changes to the layer's symbology
    layer.symbology = sym

# Function to apply symbol from gallery (for circle symbol)
def apply_circle_symbol(layer):
    sym = layer.symbology
    sym.updateRenderer("SimpleRenderer")  # Apply the SimpleRenderer for point features
    
    # Apply a circle symbol from the symbol gallery
    sym.renderer.symbol.applySymbolFromGallery("Circle 6")
    
    # Apply the changes to the layer's symbology
    layer.symbology = sym

# Function to apply 3D Billboard pushpin symbols
def apply_pushpin_symbol(layer, symbol_name):
    sym = layer.symbology
    sym.updateRenderer("SimpleRenderer")  # Apply the SimpleRenderer for point features
    
    # Apply the pushpin symbol from the gallery (Style: 3D Billboards)
    sym.renderer.symbol.applySymbolFromGallery(symbol_name)
    
    # Apply the changes to the layer's symbology
    layer.symbology = sym

# Loop through each feature class and add them to the map with appropriate symbology
for fc in feature_classes:
    fc_path = os.path.join(gdb_path, fc)  # Get the full path to the feature class
    
    if arcpy.Exists(fc_path):
        # Add the feature class to the map
        layer = map_obj.addDataFromPath(fc_path)
        
        # Apply specific symbology based on the feature class name
        if fc == "KY_CemData_Draft2013":
            # Apply "Green Pushpin 1" from 3D Billboards style
            apply_pushpin_symbol(layer, "Green Pushpin 1")
        
        elif fc == "Archaeology_HistoricCem_Centerpoints":
            # Apply "Blue Pushpin 1" from 3D Billboards style
            apply_pushpin_symbol(layer, "Blue Pushpin 1")
        
        elif fc == "KYCemPts_FindaGrave_01292025":
            # Apply Circle 6 symbol from the gallery
            apply_circle_symbol(layer)
        
        print(f"Added and symbolized {fc} to the map.")
    else:
        print(f"Feature class '{fc}' does not exist in the geodatabase.")

# Optionally, save the project after making changes
aprx.save()
```