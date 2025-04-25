# 🐍 GeoPython: Working with Geospatial Data in Python

This repository contains source files and personal practice materials based on the interactive course.   
**["Working with Geospatial Data in Python"](https://app.datacamp.com/learn/courses/working-with-geospatial-data-in-python)** from DataCamp.  

---

## 📚 Course Overview
The course covers essential geospatial techniques using Python, including:

- Introduction to geospatial data
- Working with vector and raster data
- Performing spatial joins and overlays
- Visualizing geospatial data using GeoPandas and Matplotlib

---

## 📖 Chapter Workflow Summaries

<img src="/images/Workflow.jpg" alt="Workflow" width="480">  

---


## 📖 Artisanal Mining Sites Case Study 



<details>
<summary><strong>Click here to view Code!!</strong></summary>

```python

## Import and explore the data

# Import GeoPandas and Matplotlib
import geopandas
import matplotlib.pyplot as plt

# Read the mining site data
mining_sites = geopandas.read_file("ipis_cod_mines.geojson")
national_parks = geopandas.read_file('cod_conservation.shp')

# Print the first rows and the CRS information
print(mining_sites.head(5))
print(mining_sites.crs)

print(national_parks.head(5))
print(national_parks.crs)
# Make a quick visualisation
mining_sites.plot()
national_parks.plot()
plt.show()


## Convert to common CRS and save to a file

# Plot the natural parks and mining site data
ax = national_parks.plot()
mining_sites.plot(ax=ax, color='red')
plt.show()

# Convert both datasets to UTM projection
mining_sites_utm = mining_sites.to_crs(epsg=32735)
national_parks_utm = national_parks.to_crs(epsg=32735)
# Plot the converted data again
ax = national_parks_utm.plot()
mining_sites_utm.plot(ax=ax, color='red')
plt.show()

# Write converted data to a file
mining_sites_utm.to_file("ipis_cod_mines_utm.gpkg", driver='GPKG')
national_parks_utm.to_file("cod_conservation_utm.shp", driver='ESRI Shapefile')


## Styling a multi-layered plot

# Plot of the parks and mining sites
ax = national_parks.plot(color='green')
mining_sites.plot(ax=ax, markersize=5, column = 'mineral', legend=True)
ax.set_axis_off()
plt.show()


## Buffer around a point

#set the  city of Goma as shape point
goma = Point(746989.5594829298,9816380.942287602)

# goma is a Point
print(type(goma))

# Create a buffer of 50km around Goma
goma_buffer = goma.buffer(50000)

# The buffer is a polygon
print(type(goma_buffer))


## Check how many sites are located within the buffer

mask = mining_sites.within(goma_buffer)
print(mask.sum())

# Calculate the area of national park within the buffer
intersections = national_parks.intersection(goma_buffer)
print(intersections.area.sum() / (1000**2))


## Mining sites within national parks

# Extract the single polygon for the Kahuzi-Biega National park
kahuzi = national_parks[national_parks['Name'] == "Kahuzi-Biega National park"].geometry.squeeze()

# Take a subset of the mining sites located within Kahuzi
sites_kahuzi = mining_sites[mining_sites.geometry.within(kahuzi)]
print(sites_kahuzi)

# Determine in which national park a mining site is located
sites_within_park = geopandas.sjoin(mining_sites, national_parks, op='within', how='inner')
print(sites_within_park.head())

# The number of mining sites in each national park
print(sites_within_park['Name'].count())


## Finding the name of the closest National Park

# Get the geometry of the first row
single_mine = mining_sites.geometry.loc[0]
# print(national_parks)

# Calculate the distance from each national park to this mine
dist = national_parks.geometry.distance(single_mine)

# The index of the minimal distance
idx = dist.idxmin()

# Access the name of the corresponding national park
closest_park = national_parks.loc[idx, 'Name']
print(closest_park)


## Applying a custom operation to each geometry

# Define a function that returns the closest national park

def closest_national_park(geom, national_parks):
    dist = national_parks.geometry.distance(geom)  # Cal from the mining site to all national parks
    idx = dist.idxmin()  # Find the index of the closest national park
    closest_park = national_parks.loc[idx, 'Name']
    return closest_park

# Call the function on single_mine
print(closest_national_park(single_mine, national_parks))

# Apply the function to all mining sites
mining_sites['closest_park'] = mining_sites.geometry.apply(closest_national_park,national_parks=national_parks)
print(mining_sites.head())

## Import and plot raster data

# Import the rasterio package
import rasterio

# Open the raster dataset
src = rasterio.open("central_africa_vegetation_map_foraf.tif")

# Import the plotting functionality of rasterio
import rasterio.plot

# Plot the raster layer with the mining sites
ax = rasterio.plot.show(src, cmap='terrain')
mining_sites.plot(ax=ax , color='red',markersize=1)
plt.show()


## Extract information from raster layer

# Import the rasterstats package
import rasterstats

# Extract the nearest value in the raster for all mining sites

vegetation_raster = "central_africa_vegetation_map_foraf.tif"
mining_sites['vegetation'] = rasterstats.point_query(mining_sites.geometry, vegetation_raster, interpolate='nearest')
print(mining_sites.head())

# Replace numeric vegation types codes with description
mining_sites['vegetation'] = mining_sites['vegetation'].replace(vegetation_types)

# Make a plot indicating the vegetation type
ax = mining_sites.plot(column='vegetation', legend=True,  markersize=1)
plt.show()

```
</details>  

- Note : The completed code and details can be found in [Download jupyter notebook](https://github.com/ChanikaJan/GeoPython/blob/d6827ce28f172dc7e651ee3b10be853ba6a542b7/notebooks)

### Case Studay Output :  

<img src="/images/FinalOutput01_CaseStudy.jpg?raw=true" alt="Workflow" width="480">  

<img src="/images/FinalOutput_CaseStudy.jpg?raw=true" alt="Workflow" width="480">  

---

## 🛠️ Tools & Libraries

- Python
- GeoPandas
- Rasterio
- Shapely
- Matplotlib

---

## 📁 Repository Structure

```plaintext
GeoPython/
├── data/             # Sample datasets used in the course
├── notebooks/        # Jupyter Notebooks for each chapter/topic
├── images/           # Visualizations or exported map images
└── README.md         # Project description and info
``` 

---

## 📌 Notes

This repository serves as a learning archive and reference for geospatial analysis in Python.  
All course content is credited to [DataCamp](https://www.datacamp.com/).  

### 📊 Dataset Credits

- [Paris Data open dataset](https://opendata.paris.fr/)
- [Open European Urban Atlas](https://land.copernicus.eu/en/products/urban-atlas)
- [IPIS Open Data](https://ipisresearch.be/home/maps-data/open-data/)
- [World Resources Institute (WRI)](https://www.wri.org/)




