---
title: Submission
nav_order: 7
---

# Submission
**Create a new geodatabase and copy point and polygon feature classes from the default geodatabase into it:**

*How It Works:*
- *The script uses Tkinter to prompt the user to select a folder for the new geodatabase.*
- *It checks if the new geodatabase already exists. If not, the script creates the geodatabase and copies the relevant feature classes from the default geodatabase into it.*

*This code may add the feature class copies to the current map. The copies will be listed before the originals in the Contents pane.*

```python
import arcpy
import os
import tkinter as tk
from tkinter import filedialog

# Initialize Tkinter root for file dialog
root = tk.Tk()
root.withdraw()  # Hide the root window

# Ask the user to choose a folder where to save the new geodatabase
folder_path = filedialog.askdirectory(title=f"Select a Folder to Save {county}_County.gdb")

# Check if the user selected a folder
if folder_path:
    # Define the new geodatabase name
    gdb_name = f"{county}_County.gdb"
    
    # Define the full path to the new geodatabase
    new_gdb_path = os.path.join(folder_path, gdb_name)

    # Normalize the path to use consistent slashes
    new_gdb_path = new_gdb_path.replace("\\", "/")  # Replace backslashes with forward slashes

    # Check if the geodatabase already exists, if not create it
    if not arcpy.Exists(new_gdb_path):
        arcpy.CreateFileGDB_management(folder_path, gdb_name)
        print(f"New geodatabase created: {new_gdb_path}")
    else:
        print(f"Geodatabase {gdb_name} already exists at the specified location.")

    # Get the default geodatabase from the environment
    default_gdb = arcpy.env.workspace  # This is the default geodatabase set in your ArcGIS environment

    # List all feature classes in the default geodatabase
    feature_classes = arcpy.ListFeatureClasses()

    # Check if there are feature classes to process
    if not feature_classes:
        print("No feature classes found in the default geodatabase.")
    else:
        # Loop through the feature classes
        for fc in feature_classes:
            # Check if the feature class name contains the county variable
            if county.lower() in fc.lower():  # Case-insensitive check
                try:
                    # Define the destination path for the feature class
                    destination_fc = os.path.join(new_gdb_path, fc)

                    # Normalize the path to use consistent slashes
                    destination_fc = destination_fc.replace("\\", "/")  # Replace backslashes with forward slashes

                    # Check if the feature class already exists in the new geodatabase
                    if arcpy.Exists(destination_fc):
                        print(f"Feature class {fc} already exists in the new geodatabase. Skipping...")
                    else:
                        # Copy the feature class to the new geodatabase
                        arcpy.CopyFeatures_management(fc, destination_fc)
                        print(f"Feature class {fc} copied successfully to {new_gdb_path}.")
                except Exception as e:
                    print(f"Error copying feature class {fc}: {e}")
else:
    print("No folder was selected. The process has been cancelled.")
```
**Save the project, close ArcGIS Pro, then:**

1. Navigate to the new geodatabase location using File Explorer (refer to the path in the output message above).
2. Compress the geodatabase into a ZIP file.
3. Email the ZIP file to the senior intern and/or supervisor. Please also send the quadrangles for reference.