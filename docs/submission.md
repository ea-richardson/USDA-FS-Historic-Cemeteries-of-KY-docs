f---
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

1. Navigate to the appropriate county folder in Box:
   - Open **External VSFS GIS Cemetery Mapping Project**.
   - Navigate to **[00] Inbox**.
   - Open the numbered folder corresponding to your assigned rank group (1–11), then open your assigned county folder.
   - **Do not use [01] Final or [99] Archive unless specifically directed.**

2. Upload the topographic maps:
   - Create a ZIP archive named `topos.zip` containing all `.tif` files for the assigned county.
   - Include only TIF files. Do not include Microsoft Edge HTML documents, `.ovr` files, or other unnecessary files.
   - Upload `topos.zip` directly into the county folder.

3. Create a folder named `QC1` inside the county folder.

4. Prepare and upload the geodatabase:
   - Ensure the completed geodatabase contains a single polygon feature class.
   - Compress the geodatabase into a ZIP archive.
   - Upload the ZIP archive into the `QC1` folder.

5. Verify that the submission contains the following structure:

   ```text
   [00] Inbox/
   └── [Rank Group]/
       └── [County]/
           ├── Source/              (if provided; do not modify)
           ├── topos.zip
           └── QC1/
               └── [Geodatabase].zip
   ```

6. Once QC1 has been completed by the QCer, a `QC1_Calls.zip` containing a shapefile will be added to the `QC1` folder. **Interns do not need to create this file but will populate appropriate fields during rework.**

7. Notify the appropriate personnel by email (supervisor and senior intern, if applicable) that the county assignment has been uploaded to Box and is ready for QC review. Do not attach the project files to the email; simply provide notification that the submission is available in the appropriate county folder.

8. When completing a rework (i.e., QC has been completed and corrections have been requested), create a `QC2` folder in the County folder, and place the zipped GDB with the applied calls inside. Also include a zipped copy of the QC1 shapefile with fields `Fixer` and `Response` populated.

{: .warning }
All submissions must be uploaded to Box. **Do not email assignment files.** The `Source` folder, if present, contains supplemental datasets and instructions and should not be modified unless directed.
