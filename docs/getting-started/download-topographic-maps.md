---
title: Download Topographic Maps
parent: Getting Started
nav_order: 3
---

# Download Topographic Maps
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

This project uses U.S. Geological Survey topographic maps from the 1950s (1:24,000 scale). If 1950s maps are not available, late 1940s or early 1960s maps may be substituted. Collaborators have the option to manually download GeoTIFFs from either Esri's [Historical Topo Map Explorer](https://livingatlas.arcgis.com/topomapexplorer) or USGS's [topoView](https://ngmdb.usgs.gov/topoview/viewer).

Esri's source allows for direct download of GeoTIFFs, whereas downloads from the USGS site must be extracted from a zipped folder before use. For simplicity, the script below utilizes Esri Explorer.

{: .note}
> **It may also be useful to download current topographic maps from USGS's topoView.** Newer maps may include cemetery names that were absent on older maps, especially those that have expanded considerably since the 1950s. Additionally, some cemeteries may no longer exist due to changes in the landscape, such as the creation of reservoirs. Although these changes can be observed on Google Maps or through the [KyFromAbove Program imagery](https://kygeonet.maps.arcgis.com/home/webmap/viewer.html?webmap=ba05e691cf3a4acd9583b12ccf09856e), in some cases, comparing topographic layers in ArcGIS Pro by toggling layers on and off is more effective.

## Generate URLs to directly download topos

*How It Works:*
- *Most quadrangles are named after a prominent place within the region.*
- *The script checks if the county is present in a predefined dictionary and retrieves the corresponding quadrangle names.*
- *For each quadrangle, the script queries the Nominatim API for geographic coordinates and generates an Esri map URL for the 1:24,000 scale topographic maps.*
- *It outputs the map URLs with formatting and provides instructions for downloading the maps.*

```python
import urllib.parse
import urllib.request
import urllib.error
import json
import time


# ============================================================
# TEXT FORMATTING
# ============================================================

RED = '\033[91m'
BOLD = '\033[1m'
UNDERLINE = '\033[4m'
RESET = '\033[0m'


# ============================================================
# NOMINATIM SETTINGS
# ============================================================

# Identifies this application to the Nominatim service.
# No personal email address is included.
USER_AGENT = "KentuckyTopoMapResearch/1.0"

# Nominatim search API
NOMINATIM_URL = "https://nominatim.openstreetmap.org/search?"

# Wait at least one second between requests to the
# public Nominatim service.
REQUEST_DELAY = 1.0

# Store coordinates already found during this notebook
# session so the same place does not need to be queried again.
coordinate_cache = {}


# ============================================================
# FUNCTION: GET COORDINATES
# ============================================================

def get_coordinates(place_name):
    """
    Get latitude and longitude for a Kentucky quadrangle
    using the Nominatim OpenStreetMap geocoding service.
    """

    # --------------------------------------------------------
    # Make sure the search is restricted to Kentucky
    # --------------------------------------------------------

    if ", KY" not in place_name.upper():
        search_name = f"{place_name}, KY"
    else:
        search_name = place_name

    # --------------------------------------------------------
    # Check whether this location has already been geocoded
    # --------------------------------------------------------

    if search_name in coordinate_cache:
        return coordinate_cache[search_name]

    # --------------------------------------------------------
    # Nominatim search parameters
    # --------------------------------------------------------

    params = {
        "q": search_name,
        "format": "jsonv2",
        "addressdetails": 1,
        "limit": 1,
        "countrycodes": "us"
    }

    # Construct the complete URL
    url = NOMINATIM_URL + urllib.parse.urlencode(params)

    # --------------------------------------------------------
    # Create request with identifying User-Agent
    # --------------------------------------------------------

    request = urllib.request.Request(
        url,
        headers={
            "User-Agent": USER_AGENT
        }
    )

    # --------------------------------------------------------
    # Send request to Nominatim
    # --------------------------------------------------------

    try:

        with urllib.request.urlopen(
            request,
            timeout=30
        ) as response:

            data = json.load(response)

        # ----------------------------------------------------
        # Process successful response
        # ----------------------------------------------------

        if data:

            lat = data[0]["lat"]
            lon = data[0]["lon"]

            # Save result in cache
            coordinate_cache[search_name] = (lat, lon)

            return lat, lon

        else:

            print(
                f"{RED}"
                f"No coordinates found for: "
                f"{search_name}"
                f"{RESET}"
            )

            coordinate_cache[search_name] = (None, None)

            return None, None

    # --------------------------------------------------------
    # Handle HTTP errors such as 403
    # --------------------------------------------------------

    except urllib.error.HTTPError as e:

        print(
            f"{RED}"
            f"Nominatim HTTP error for "
            f"'{search_name}': "
            f"{e.code} {e.reason}"
            f"{RESET}"
        )

        # Attempt to display the server's response
        try:

            error_message = e.read().decode(
                "utf-8",
                errors="replace"
            )

            if error_message:

                print(
                    f"Server response: "
                    f"{error_message[:500]}"
                )

        except Exception:
            pass

        return None, None

    # --------------------------------------------------------
    # Handle network errors
    # --------------------------------------------------------

    except urllib.error.URLError as e:

        print(
            f"{RED}"
            f"Network error for "
            f"'{search_name}': "
            f"{e.reason}"
            f"{RESET}"
        )

        return None, None

    # --------------------------------------------------------
    # Handle unexpected errors
    # --------------------------------------------------------

    except Exception as e:

        print(
            f"{RED}"
            f"Unexpected error for "
            f"'{search_name}': "
            f"{e}"
            f"{RESET}"
        )

        return None, None


# ============================================================
# FUNCTION: GENERATE ESRI MAP LINK
# ============================================================

def generate_map_link(lat, lon):
    """
    Generate an Esri Topographic Map Explorer URL
    centered on the supplied coordinates.
    """

    esri_url = (
        f"https://livingatlas.arcgis.com/topomapexplorer/"
        f"#maps=&loc={lon},{lat}&LoD=15.76"
    )

    return esri_url


# ============================================================
# MAIN PROCESS
# ============================================================

# Check whether the county exists in the countyquads dictionary

if county in countyquads.keys():

    quads = countyquads[county]

    # --------------------------------------------------------
    # Introductory message
    # --------------------------------------------------------

    print(
        f"{RED}"
        f"Please open the URL for each quadrangle, "
        f"filter for the 24k (1:24,000) scale, and "
        f"download the appropriate topographic map "
        f"for the year you want (~1950s)."
        f"{RESET}"
    )

    print(
        f"{BOLD}"
        f"{county} County requires "
        f"{UNDERLINE}{len(quads)}{RESET}{BOLD} "
        f"topographic maps:"
        f"{RESET}"
    )

    print()

    # --------------------------------------------------------
    # Process each quadrangle
    # --------------------------------------------------------

    for i, quad in enumerate(quads, start=1):

        print(
            f"{BOLD}"
            f"[{i}/{len(quads)}] {quad}"
            f"{RESET}"
        )

        # Get coordinates from Nominatim
        lat, lon = get_coordinates(quad)

        # ----------------------------------------------------
        # Coordinates successfully found
        # ----------------------------------------------------

        if lat is not None and lon is not None:

            # Generate Esri Topo Map Explorer URL
            esri_url = generate_map_link(lat, lon)

            print(
                f"    {esri_url}"
            )

        # ----------------------------------------------------
        # Coordinates not found
        # ----------------------------------------------------

        else:

            print(
                f"    {RED}"
                f"Coordinates not found. "
                f"Please manually search for "
                f"'{quad}' in the Esri Topo Map Explorer."
                f"{RESET}"
            )

        # ----------------------------------------------------
        # Wait between requests
        #
        # Do not wait after the final quadrangle.
        # ----------------------------------------------------

        if i < len(quads):

            time.sleep(REQUEST_DELAY)

    print()

    print(
        f"{BOLD}"
        f"Finished processing {len(quads)} quadrangles."
        f"{RESET}"
    )

# ============================================================
# COUNTY NOT FOUND
# ============================================================

else:

    print(
        f"{RED}"
        f"County not found in the predefined list.\n"
        f"Please make sure the county name is correct."
        f"{RESET}"
    )
```
