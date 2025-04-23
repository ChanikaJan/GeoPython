# 🐍 GeoPython: Working with Geospatial Data in Python

This repository contains source files and personal practice materials based on the interactive course.   
**["Working with Geospatial Data in Python"](https://app.datacamp.com/learn/courses/working-with-geospatial-data-in-python)** from DataCamp.

## 📚 Course Overview
The course covers essential geospatial techniques using Python, including:

- Introduction to geospatial data
- Working with vector and raster data
- Performing spatial joins and overlays
- Visualizing geospatial data using GeoPandas and Matplotlib

## 📖 Chapter Summaries 
<details>
<summary><strong>Chapter 01 - Introduction to Geospatial Vector Data</strong></summary>

```python
import geopandas as gpd

# Load a shapefile
gdf = gpd.read_file("data/neighborhoods.shp")

# Plot it
gdf.plot()
```
</details>



Chapter 02 - Spatial Relationshipsv  
Chapter 03 - Projecting and Transforming Geometries  
Chapter 04 - Putting It All Together – Artisanal Mining Sites Case Study    


## 🛠️ Tools & Libraries

- Python
- GeoPandas
- Rasterio
- Shapely
- Matplotlib

## 📁 Repository Structure

```plaintext
GeoPython/
├── data/             # Sample datasets used in the course
├── notebooks/        # Jupyter Notebooks for each chapter/topic
├── images/           # Visualizations or exported map images
└── README.md         # Project description and info
``` 

## 📌 Notes

This repository serves as a learning archive and reference for geospatial analysis in Python.  
All course content and datasets are credited to [DataCamp](https://www.datacamp.com/).





