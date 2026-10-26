---
title: Attribution
nav_order: 5
---

# Attribution
{: .no_toc }

While the reliability and completeness of these sources vary, they collectively provide a strong foundation for initial cemetery mapping efforts.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Automated
### Populate the *County_Name* field

```python
arcpy.management.CalculateField(
    in_table=f"{county}_County",
    field="County_Name",
    expression=f'"{county}"',
    expression_type="PYTHON3",
    code_block="",
    field_type="TEXT",
    enforce_domains="NO_ENFORCE_DOMAINS"
)
```

### Populate the *Latitude* and *Longitude* fields

```python
arcpy.management.CalculateGeometryAttributes(
    in_features=f"{county}_County",
    geometry_property="Latitude CENTROID_Y;Longitude CENTROID_X",
    length_unit="",
    area_unit="",
    coordinate_system=None,
    coordinate_format="SAME_AS_INPUT"
)
```

### Populate the *Zip* field using [U.S. Postal Service ZIP Code Boundaries](https://www.arcgis.com/home/item.html?id=5f31109b46d541da86119bd4cf213848)

*How It Works:*
- *The script checks if the *zip_zones* feature class exists in the geodatabase and performs a spatial join.*
- *It searches for a field with "AddSpatialJoin" in its name and "ZIP Code" in its alias to dynamically identify the relevant field for calculation.*
- *After calculating the field values, the script removes the spatial join and prints status messages throughout the process.*

```python
import arcpy
import os

# Set the workspace to the correct geodatabase path
arcpy.env.workspace = f"{gdb_path}"  # Adjust the path accordingly

# Check if "zip_zones" exists in the workspace
zip_zones_path = os.path.join(arcpy.env.workspace, "zip_zones")
print(f"Checking if {zip_zones_path} exists...")
if arcpy.Exists(zip_zones_path):
    print(f"{zip_zones_path} exists.")
else:
    print(f"ERROR: {zip_zones_path} does not exist.")
    raise Exception(f"ERROR: {zip_zones_path} does not exist.")

# Proceed if 'zip_zones' exists
print(f"Starting spatial join for {county}_County with zip_zones...")

arcpy.management.AddSpatialJoin(
    target_features=f"{county}_County",
    join_features=zip_zones_path,  # Ensure it's the full path
    join_operation="JOIN_ONE_TO_ONE",
    join_type="KEEP_ALL",
    field_mapping='ZIP_CODE "ZIP Code" true true false 10 Text 0 0,First,#,zip_zones,ZIP_CODE,0,9;PO_NAME "Post Office Name" true true false 100 Text 0 0,First,#,zip_zones,PO_NAME,0,99;Shape_Length "Shape_Length" false true true 8 Double 0 0,First,#,zip_zones,Shape_Length,-1,-1;Shape_Area "Shape_Area" false true true 8 Double 0 0,First,#,zip_zones,Shape_Area,-1,-1',
    match_option="HAVE_THEIR_CENTER_IN",
    search_radius=None,
    distance_field_name="",
    permanent_join="NO_PERMANENT_FIELDS",
    match_fields=None
)

# List all fields in the table
fields = arcpy.ListFields(f"{county}_County")
print(f"Fields in {county}_County: {', '.join([field.name for field in fields])}")

# Initialize a variable to hold the field name
zip_code_field = None

# Loop through the fields and check for the conditions
print(f"Searching for the field with 'AddSpatialJoin' in the name and 'ZIP Code' in the alias...")
for field in fields:
    # Check if the field name contains "AddSpatialJoin" and the alias contains "ZIP Code"
    if "AddSpatialJoin" in field.name and "ZIP Code" in field.aliasName:
        zip_code_field = field.name
        print(f"Found matching field: {zip_code_field}")
        break  # Exit the loop once we find the matching field

# Check if we found the correct field
if zip_code_field is None:
    print("ERROR: Field with 'AddSpatialJoin' in the name and 'ZIP Code' in the alias not found.")
    raise Exception("Field with 'AddSpatialJoin' in the name and 'ZIP Code' in the alias not found.")

# Print out the field name for debugging purposes
print(f"ZIP code field identified: {zip_code_field}")

# Now use the dynamically found field name in the CalculateField expression
print(f"Calculating field values for {county}_County.Zip using field {zip_code_field}...")
arcpy.management.CalculateField(
    in_table=f"{county}_County",
    field=f"{county}_County.Zip",  # Assuming you want to populate the 'Zip' field in your table
    expression=f"!{zip_code_field}!",  # Use the dynamically found field name
    expression_type="PYTHON3",
    code_block="",
    field_type="TEXT",
    enforce_domains="NO_ENFORCE_DOMAINS"
)

# Remove spatial join
join_name = zip_code_field.split('.')[0]  # Assuming split at period is a safe approach for the join name
print(f"Removing spatial join: {join_name} from {county}_County")

arcpy.management.RemoveJoin(
    in_layer_or_view=f"{county}_County",
    join_name=f"{join_name}"
)

# Optional: Inform user that the process is completed
print("Spatial join and field calculation completed successfully. Spatial join removed.")
```

## Manual

### Populate remaining attribute fields
(*Cemetery_Name1*, *Cemetery_Name2*, *Street_Address*, *City*, *Comments*, *Burial_Number*, *Link*)

Note that some attribute fields may not have valid values available. If you are unable to find a valid value, leave the field blank and/or make a note in the "Comments" field.

Sources:
- [Google Maps](https://www.google.com/maps)
- [OpenStreetMap](https://www.openstreetmap.org/)
- [Kentucky Natural Color Imagery (2024)](https://kygeonet.maps.arcgis.com/home/webmap/viewer.html)
- *KYCemPts_FindaGrave_01292025* feature class
- Find a Grave [website](https://www.findagrave.com/cemetery)
- [Regrid parcel data](https://app.regrid.com/us/ky)
- [Cemeteries in Kentucky Database](https://kyhistory.com/digital/collection/LIB/id/493/)

*See additional sources in the Sources section.*

Where possible, each digitized cemetery should be matched to an existing Find a Grave cemetery record. The purpose is to connect the historical GIS record with an existing modern cemetery record.

When a match is found:
* record the appropriate cemetery name information;
* record the burial count when available;
* record the Find a Grave URL in `Link`;
* use the information as supporting evidence.

{: .warning }
> `Burial_Number` is intended to record available burial information. A burial count from Find a Grave should not automatically be interpreted as the total historical number of people buried at a given location. If burial counts conflict, assign the larger number to this field and note the discrepancy in `Comments`.

{: .note }
> If a Find a Grave record appears to correspond to the historical cemetery but the match is uncertain: (1) document the *potential* match in `Comments`; (2) preserve the source information; and (3) flag unusual or ambiguous cases for review.

Use your own judgment in assigning point attributes to digitized polygon features. **The Find a Grave points may or may not overlap with digitized cemeteries; in some cases, a point may not overlap but will be in close proximity to the polygon.**

Even if no Find a Grave point is near a cemetery of interest, Google Maps, OpenStreetMap, or Regrid parcel data may show a cemetery name. If this occurs, you may be able to find a Find a Grave match by searching the website and matching based on a unique cemetery name, address, or description. (Even potential matches can be linked in the "Comments" field.)

### Input your search term and automatically open the Find a Grave search URL in your default browser

```python
import webbrowser

# Define the red color using ANSI escape codes
RED = '\033[91m'
RESET = '\033[0m'  # Reset color back to default

# Create a dictionary that maps counties to their IDs
county_to_id = {"Adair": 989, "Allen": 990, "Anderson": 991,
                "Ballard": 992, "Barren": 993, "Bath": 994, "Bell": 995, "Boone": 996, "Bourbon": 997, "Boyd": 998, "Boyle": 999, "Bracken": 1000, "Breathitt": 1001, "Breckinridge": 1002, "Bullitt": 1003, "Butler": 1004,
                "Caldwell": 1005, "Calloway": 1006, "Campbell": 1007, "Carlisle": 1008, "Carroll": 1009, "Carter": 1010, "Casey": 1011, "Christian": 1012, "Clark": 1013, "Clay": 1014, "Clinton": 1015, "Crittenden": 1016, "Cumberland": 1017,
                "Daviess": 1018,
                "Edmonson": 1019, "Elliott": 1020, "Estill": 1021,
                "Fayette": 1022, "Fleming": 1023, "Floyd": 1024, "Franklin": 1025, "Fulton": 1026,
                "Gallatin": 1027, "Garrard": 1028, "Grant": 1029, "Graves": 1030, "Grayson": 1031, "Green": 1032, "Greenup": 1033,
                "Hancock": 1034, "Hardin": 1035, "Harlan": 1036, "Harrison": 1037, "Hart": 1038, "Henderson": 1039, "Henry": 1040, "Hickman": 1041, "Hopkins": 1042,
                "Jackson": 1043, "Jefferson": 1044, "Jessamine": 1045, "Johnson": 1046,
                "Kenton": 1047, "Knott": 1048, "Knox": 1049,
                "LaRue": 1050, "Laurel": 1051, "Lawrence": 1052, "Lee": 1053, "Leslie": 1054, "Letcher": 1055, "Lewis": 1056, "Lincoln": 1057, "Livingston": 1058, "Logan": 1059, "Lyon": 1060,
                "Madison": 1064, "Magoffin": 1065, "Marion": 1066, "Marshall": 1067, "Martin": 1068, "Mason": 1069, "McCracken": 1061, "McCreary": 1062, "McLean": 1063, "Meade": 1070, "Menifee": 1071, "Mercer": 1072, "Metcalfe": 1073, "Monroe": 1074, "Montgomery": 1075, "Morgan": 1076, "Muhlenberg": 1077,
                "Nelson": 1078, "Nicholas": 1079,
                "Ohio": 1080, "Oldham": 1081, "Owen": 1082, "Owsley": 1083,
                "Pendleton": 1084, "Perry": 1085, "Pike": 1086, "Powell": 1087, "Pulaski": 1088,
                "Robertson": 1089, "Rockcastle": 1090, "Rowan": 1091, "Russell": 1092,
                "Scott": 1093, "Shelby": 1094, "Simpson": 1095, "Spencer": 1096,
                "Taylor": 1097, "Todd": 1098, "Trigg": 1099, "Trimble": 1100,
                "Union": 1101,
                "Warren": 1102, "Washington": 1103, "Wayne": 1104, "Webster": 1105, "Whitley": 1106, "Wolfe": 1107, "Woodford": 1108}

# Print instructions in red
print(f"{RED}Note: Upper or lowercase does not matter.\nYou can enter any part of the cemetery name (partial or full).\nFor example, to search for 'Pleasant Valley Memorial Gardens', you could enter 'pleasant', 'valley', or 'gardens'.{RESET}")

# Prompt the user for the search term (cemetery name)
search_term = input("Please enter your search term: ")

# Find the county ID by matching the county name (case-insensitive)
county_id = None
for key, value in county_to_id.items():
    if key.lower() == county.lower():  # Case-insensitive match
        county_id = value
        break

# If the county is not found, print an error message
if county_id is None:
    print(f"{RED}County not found! Please ensure the county name is correct.{RESET}")
else:
    # Format the URL with the user's input and the selected county ID
    county_encoded = county.replace(" ", "+")  # URL encode spaces
    url = f"https://www.findagrave.com/cemetery/search?cemetery-name={search_term.replace(' ', '+')}&cemetery-loc={county_encoded}+County%2C+Kentucky%2C+USA&only-with-cemeteries=cemOnly&locationId=county_{county_id}"
    
    # Open the URL in the default web browser
    webbrowser.open(url)
```

### If you want to check unmatched/unknown cemeteries for headstones, nearby landmarks, or addresses in Google Maps

*Run this AFTER copying the approximate centroid coordinates of a digitized polygon. (Right-click the center of the cemetery in the map, select "Convert Coordinates", then select "DD".)*

```python
import webbrowser, tkinter

def visual_check():
    win = tkinter.Tk()
    win.withdraw()
    coordinates = win.clipboard_get()
    webbrowser.open(f"https://www.google.com/maps/place/{coordinates}")    

visual_check()
```

### Comments
This field should be used to provide context and preserve information that does not fit cleanly into the structured fields. Be detailed yet concise.

Examples include:
* alternate cemetery names;
* uncertain Find a Grave matches;
* source conflicts;
* lack of a mapped cemetery boundary;
* use of the 0.25-acre default polygon;
* unusual historical map circumstances;
* additional source information;
* issues requiring later review.

**Good comment:** Cemetery sourced from USFS 2013 points layer; 0.25-acre default polygon used. Potential Find a Grave record \[corresponding URL\] corresponding URL but match is not confirmed.

**Poor comment:** Cemetery maybe here.
