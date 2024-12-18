# Coding Crossover: Leveraging R Skills in Python
This repository provides a side-by-side comparison of how to perform common geomatics tasks in **R** and **Python**. It focuses on the libraries `terra` and `sf` in R, and `rasterio`, `numpy`, `pandas`, and `geopandas` in Python.

---
## Key Differences in Python vs. R
There are some key differences between Python and R that may take some time to get used to when switching between them.

- **Indexing**
  - Python uses zero-based indexing (lists, arrays start from index 0).
  - R uses one-based indexing (vectors, matrices start from index 1).
- **Data structures**
  - Python's primary sequence structures are lists (mutable) and tuples (immutable), and numpy arrays for numerical operations.
  - R's core structures are vectores, matrices, data frames and lists, where vectors are a fundamental unit of operation.
- **Vectorization and broadcasting**
  - R is inherently vectorized; many operations naturally apply element-wise without extra effort.
  - Python requires libraries like `NumPy` for similar vectorized operations and broadcasting.
- **Function arguments**
  - In Python, keyword arguments (kwargs) are passed by name after positional arguments, and default values are common.
  - In R, arguments can be matched by position or name, and partial argument matching (unique abbreviations) is allowed.
- **Assignment**
  - Python uses `=` for assignment.
  - R commonly uses `<-` for assignment, though `=` can also be used.
- **Looping and iteration**
  - Python encourages explicit loops (e.g., `for`, `while`), and list comprehension is widely used.
      `squares = [x**2 for x in range(10)] # example list comprehension`
  - R encourages vectorized operations and `apply` functions over explicit `for` loops for efficiency and clarity.
- **String handling**
  - Python has robust built-in string operations, slicing and methods.
  - R relies on more external packages (like stringr) for advanced text manipulation, though basic operations are available natively.

---
## Common Geomatics Libraries

### R Libraries
| **Library**   | **Description**                                                                                   |
|---------------|---------------------------------------------------------------------------------------------------|
| **`terra`**   | For spatial data manipulation, raster data processing, and geospatial analysis.                  |
| **`sf`**      | For handling vector spatial data, including shapefiles, GeoJSON, and other formats.              |
| **`dplyr`**   | For data manipulation.                                                                           |
| **`caret`**   | For training and plotting classification and regression models.                                  |
| **`ggplot2`** | For creating graphics with provided data.                                                        |

### Python Libraries
| **Library**        | **Description**                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------|
| **`rasterio`**     | For raster file I/O and processing.                                                              |
| **`numpy`**        | For numerical data manipulation, often used with raster data.                                    |
| **`pandas`**       | For tabular data manipulation.                                                                   |
| **`geopandas`**    | For handling vector spatial data, extending `pandas` to work with geospatial formats.            |
| **`shapely`**      | For manipulation and analysis of geometric objects.                                              |
| **`scikit-learn`** | For training classification, regression, and clustering models, and data preprocessing.          |
| **`matplotlib`**   | For creating graphics with provided data.                                                        |

## Importing Libraries

### R
```R
# Import libraries
library(terra)   # For raster operations
library(sf)      # For vector operations
library(dplyr)   # For data manipulation
library(ggplot2) # For creating visualizations
```

### Python
```python
# Import libraries
import rasterio                                                     # For raster operations
import geopandas as gpd                                             # For vector data
import numpy as np                                                  # For numerical data
import pandas as pd                                                 # For tabular data manipulation
import matplotlib.pyplot as plt                                     # For creating visulizations
from rasterio.features import rasterize                             # For rasterizing vector data
from rasterio.mask import mask                                      # For masking raster data
from rasterio.warp import calculate_default_transform, reproject    # For raster reprojection
from shapely.geometry import box                                    # For creating bounding box
```

---
## Cheat Sheet: Common Geomatics Functions

| **Functionality**            | **R (`terra`, `sf`)**                                                                                                                                   | **Python (`rasterio`, `geopandas`, etc.)**                                                                                                                      |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Read a shapefile**          | `shp <- sf::st_read("path/to/file.shp")`                                                                                                               | `shp = gpd.read_file("path/to/file.shp")`                                                                                                                      |
| **Write a shapefile**         | `sf::st_write(shp, "path/to/output.shp")`                                                                                                              | `shp.to_file("path/to/output.shp")`                                                                                                                            |
| **Read a raster**             | `raster <- terra::rast("path/to/file.tif")`                                                                                                            | `with rasterio.open("path/to/file.tif") as src:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`raster = src.read()`                                                              |
| **Write a raster**            | `terra::writeRaster(raster, "path/to/output.tif", overwrite=TRUE)`                                                                                     | `with rasterio.open("path/to/output.tif", "w", **kwargs) as dst:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`dst.write(raster)`                                               |
| **Calculate NDVI**            | `ndvi <- (nir - red) / (nir + red)` <br>*(assuming `nir` and `red` are `terra` raster objects)*                                                        | `ndvi = (nir - red) / (nir + red)` <br>*(assuming `nir` and `red` are `numpy` arrays)*                                                                         |
| **Clip a raster by extent**   | `clipped <- terra::crop(raster, extent)`                                                                                                               | `clipped, _ = mask(src, shapes, crop=True)`                                                                                |
| **Clip a vector by extent**   | `clipped <- sf::st_crop(vector, xmin = x1, ymin = y1, xmax = x2, ymax = y2)`                                                                            | `bbox = box(x1, y1, x2, y2)`<br>`clipped = vector.clip(bbox)`                                                            |
| **Reproject a shapefile**     | `reproj <- sf::st_transform(shp, crs = 4326)`                                                                                                          | `shp = shp.to_crs(epsg=4326)`                                                                                                                                 |
| **Reproject a raster**        | `reproj <- terra::project(raster, "EPSG:4326")`                                                                                                        | `reprojected_raster = reproject(src, transform)`                                        |
| **Calculate area of polygons**| `shp$area <- sf::st_area(shp)`                                                                                                                         | `shp["area"] = shp.geometry.area`                                                                                                                              |
| **Sort polygons by attribute**| `sorted <- shp[order(shp$attribute), ]`                                                                                                                | `sorted = shp.sort_values("attribute")`                                                                                                                        |
| **Extract raster values**     | `values <- terra::extract(raster, sf::st_coordinates(points))`                                                                                         | `values = [raster[row, col] for row, col in points]` *(requires array coordinates for `numpy`)*                                                                |
| **Buffer around features**    | `buffered <- sf::st_buffer(shp, dist = 500)`                                                                                                           | `buffered = shp.buffer(500)`                                                                                                                                  |
| **Rasterize a vector layer**  | `rasterized <- terra::rasterize(shp, raster)`                                                                                                          | `rasterized = rasterize([(geom, 1) for geom in shp.geometry], out_shape=shape, transform=transform)`             |

---
## Classification Example
In GEM520 there was a lab assignment that focused on how to perform a supervised image classification using QGIS and R to represent the 7 land cover classes for the Gulf Islands. Within this lab training and validation polygons were delineated using QGIS and then used to train a Maximum Likelihood Classifier in R. In this example we will be using those same polygons and Landsat image but we will be doing the classification step using Python instead.

*Note: it is recommended that you use the same polygons that you delineated for the lab to see if there are any differences in the outputs, but if you do not have them still you can access them [here](https://github.com/ubc-mgem-fcor599/FCOR-599-Workshops/tree/main/CodingCrossover/data).*

### Step 1
Before being able to run this example you will need to set up a Conda environment with the correct packages installed. To do this you will first need to download the `create_environment.py` script from [here](https://github.com/ubc-mgem-fcor599/FCOR-599-Workshops/blob/main/CodingCrossover/scripts/create_environment.py).

Next you need to open up Anaconda Prompt and type in `python [path/to/create_environment.py]`. This will create a new Conda environment for you with the correct packages and then open a new Jupyter Lab IDE.

*Note: you do not need to run this example in the Jupter Lab IDE but the live tutorial will be done using it.*

### Step 2
Once the environment is created and Jupyter Lab is opened you can download the `supervised_classification.ipynb` from [here](https://github.com/ubc-mgem-fcor599/FCOR-599-Workshops/blob/main/CodingCrossover/scripts/supervised_classification.ipynb). This notebook will walk you through similar steps to the ones found in the original lab assignment. Open this notebook and run each of the code chunks.

*Note: you will need to change the file paths within this notebook to the ones in your system and run each code chunk to show your results.*
