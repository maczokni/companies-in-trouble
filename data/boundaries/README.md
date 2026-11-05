# MSOA boundaries (download)

The analysis uses the full-resolution 2011 MSOA boundaries for England (`england_msoa_2011.shp`, with its `.shx`, `.dbf` and `.prj` files). They are open data, but at about 245 MB they are too large for GitHub.

## How to download them

1. Go to the UK Data Service boundary data selector: <https://borders.ukdataservice.ac.uk/>. If the link has moved, search the UK Data Service website for "Census boundary data".
2. Choose **England**, **2011 and later**, and the dataset **English Middle Layer Super Output Areas, 2011**.
3. Select all areas and download the shapefile.
4. Unzip it and put the four files in this folder, so that the code finds `data/boundaries/england_msoa_2011.shp`.

The files need the columns `code` (MSOA code) and `name`, as in the UK Data Service download.

**Why these boundaries.** The spatial neighbours (queen contiguity) are built from these full-resolution boundaries. Generalised versions from the ONS Open Geography Portal would give a slightly different neighbour structure, and so slightly different spatial results.

**Licence and citation.** Open Government Licence v3. Office for National Statistics (2011). *2011 Census: boundary data (England and Wales)* [data collection]. UK Data Service. SN:5819 UKBORDERS: Digitised Boundary Data, 1840- and Postcode Directories, 1980-. Contains National Statistics data © Crown copyright and database right. Contains OS data © Crown copyright and database right.
