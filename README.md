# photo_mgmt
Tools used for photo management.

# Installation
Users will need [Python 3](https://www.python.org/) for the python script and [R](https://www.r-project.org/) for the R script
For the python script please use pip and the distributed requirements.txt file. 

`pip3 install -r requirements.txt`

You will also need to install the [spatialite extension module](https://www.gaia-gis.it/fossil/libspatialite/index) *(mod_spatialite)* and add it to your system path if you want geometry support.

For the R script, the required libraries are at the beginning of the R script, and users will have to check and install.packages() manually. The spatialite is required for the R scripts currently.

# Usage
## Scanning a path for images
`python3 ./scan_image.py -h`

`Rscript ./scan_image.R -h`

The scan_photo script will recursively scan a directory for photos, calculate their md5 hash and extract their EXIF tags for storage in a SQLite database. If the --geo flag is given in the python script, the spatialite extension is loaded for the database creation process and any geometry stored in the EXIF tags are stored in the *location* table with appropriate geometry. 

The python script stores the EXIF tags in a long format while the R script stores them in a wide format, though essentially both scripts do the same thing.

### Database Structure
**import:**
This table stores import, update, and renaming data.

**hash:**
This table stores md5 hash data and import dates for each unique hash.

**photo:**
This table stores file paths of found images and their md5 hash, duplicates are allowed but file paths, of course, are unique.

**tag:**
This table stores image metadata stored in the EXIF tags (if they exist).

## Renaming images
`python3 rename.py -h`

This script will take various options and flags and update the path in the database. If both the new path and old path are valid, the script will then rename the files, creating folders as needed.

**WARNING**

It is very easy to accidently move your files to the wrong place. Use the --test flag liberally and pay attention to the random examples generated at the confirmation prompt.

## Reconnecting lost files ##
`python3 update.py -h`

This script will scan a file path for images and update the *photo* table with the new path if one is found. In the case of duplicate files sharing the same md5 hash but different filenames, the first old path will be updated with the first new path sharing a hash, the second with the second, etc.

This script is useful for reconnecting borken paths after manual moving or renaming has been since the creation of the database.

# Contributing
If you want to add functionality or error checking features to anything here, please feel free to contact the author.

# Credits 
Author: Wade Lieurance

# License 
Gnu Public License v3