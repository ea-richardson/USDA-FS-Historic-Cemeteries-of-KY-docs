---
title: Directory Setup
parent: Getting Started
nav_order: 2
---

# Directory Setup
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Create a new directory structure

This will be inside an existing "topos" directory or within a newly created "topos" directory and format the subdirectory names based on the current date and county name:

*How It Works:*
- *Prompts the user to decide if an existing "topos" directory should be used or a new one should be created.*
- *If an existing "topos" directory is chosen, it generates a subdirectory with a formatted name based on the county name, current date, and the next available number.*
- *If a new "topos" directory is created, the script sets up the "topos" directory first and then creates the same county subdirectory with the appropriate naming convention.*

```python
import os
import tkinter as tk
from tkinter import filedialog
from datetime import datetime

def format_dir_name(name):
    """Format directory names by replacing spaces with hyphens and commas with underscores."""
    name = name.lower().replace(', ', '_').replace(' ', '-')
    name = name.replace('-_', '_') # Ensure no mixed characters like "-_"
    return name

def get_next_directory_number(base_directory):
    """Determine the next available directory number in the given base directory."""
    existing_dirs = os.listdir(base_directory)
    existing_numbers = []

    # Loop through existing directories and find those starting with a number
    for directory in existing_dirs:
        # Check if the directory starts with a number and a hyphen (e.g., "01-whitley")
        if directory[0].isdigit() and '-' in directory:
            # Extract the number from the beginning of the directory name
            number_part = directory.split('-')[0]
            if number_part.isdigit():
                existing_numbers.append(int(number_part))
    
    # Find the next available number (starting from 1)
    next_number = 1
    if existing_numbers:
        next_number = max(existing_numbers) + 1

    # Format the number as two digits (01, 02, etc.)
    return f"{next_number:02d}"

def get_current_date():
    """Return the current date formatted as MMDDYYYY."""
    return datetime.now().strftime("%m%d%Y")

# Initialize GUI file dialog
root = tk.Tk()
root.withdraw()

directory_order = input('Do you have an existing "topos" directory? (y/n) ').lower()

current_date = get_current_date()

if directory_order == "y":
    print(f'Select the existing "topos" directory.')
    topo_directory = filedialog.askdirectory()
    
    # Format the county name
    formatted_county = format_dir_name(county)
    
    # Get the next available number for the county directory
    next_number = get_next_directory_number(topo_directory)
    new_county_directory_name = f"{next_number}-{formatted_county}_{current_date}"
    county_directory = os.path.join(topo_directory, new_county_directory_name)
    
    # Create the county subdirectory
    os.makedirs(county_directory, exist_ok=True)
    print(f'Success! Created: "{os.path.normpath(county_directory)}')

elif directory_order == "n":
    print(f'Select the parent directory to create a new "topos" directory.')
    directory_path = filedialog.askdirectory()

    # Create "topos" directory
    topo_directory = os.path.join(directory_path, "topos")
    os.makedirs(topo_directory, exist_ok=True)
    print(f'Subdirectory "topos" created at: {os.path.normpath(topo_directory)}.')
    
    # Format the county name
    formatted_county = format_dir_name(county)
    
    # Get the next available number for the county directory
    next_number = get_next_directory_number(topo_directory)
    new_county_directory_name = f"{next_number}-{formatted_county}_{current_date}"
    county_directory = os.path.join(topo_directory, new_county_directory_name)
    
    # Create the county subdirectory
    os.makedirs(county_directory, exist_ok=True)
    print(f'Subdirectory "{os.path.normpath(county_directory)}" created.')

else:
    print("Invalid input. Please re-run and enter 'y' or 'n'.")
```

## Connect the "topos" folder to the project

```python
import arcpy
import os

# Get the path of the currently open ArcGIS Pro project (if there is one open)
aprx = arcpy.mp.ArcGISProject("CURRENT")  # 'CURRENT' refers to the active project

# Check if the project is open
if not aprx:
    print("No project is currently open.")
else:
    # Get the current folder connections
    folder_connections = aprx.folderConnections

    # Define the new folder path using os.path.normpath
    new_folder = os.path.normpath(topo_directory)  # Normalize the path
    alias = "Topos Folder"  # Alias for the new folder connection
    is_home_folder = False  # Set to True if this should be the default home folder

    # Create a dictionary for the new folder connection
    new_folder_dict = {
        "connectionString": new_folder,
        "alias": alias,
        "isHomeFolder": is_home_folder
    }

    # Add the new folder connection to the list
    folder_connections.append(new_folder_dict)

    # Optional: Validate the folder path (ensure the folder exists if validate=True)
    validate = True

    # Update the folder connections in the project
    aprx.updateFolderConnections(folder_connections, validate)

    # Save the project after the update
    aprx.save()

    print("Folder connection updated successfully!")
```
