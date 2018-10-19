# Google-Maps-Python
How to Generate a Geographical Heatmap with Python
# Let's get started
This tutorial will show you how to create an interactive heatmap overlaying Google maps. The end result will be an HTML file that you can open and zoom in/out of or pan through to visualize your customer addresses. Our end result will look like this.

For this tutorial we will be using the gmplot library. More information and requirements can be found here.

To install use pip install gmplot

The dataset we will be using dataset from mysql databases.

```
#!/usr/bin/python3

# Import the necessary libraries
import pandas as pd
import gmplot
import pymysql
import csv
import googlemaps

# Open database connection
db = pymysql.connect("10.200.40.28","tats","tats","campaign_analysis" )

# prepare a cursor object using cursor() method
cursor = db.cursor()

# execute SQL query using execute() method.
cursor.execute("SELECT lat,lon from impression")

# Fetch a single row using fetchone() method.
result = cursor.fetchall()

c = csv.writer(open("coordinate.csv","w"))
c.writerow(['lat','lon'])
for data in result:
    c.writerow(data)
raw_data = pd.read_csv("coordinate.csv")

# Let's limit the dataset to the first 15,000 records for this example
data = raw_data.head(n=15000)

# Store our latitude and longitude
latitudes = data["lat"]
longitudes = data["lon"]

# Creating the location we would like to initialize the focus on.
# Parameters: Lattitude, Longitude, Zoom
gmap = gmplot.GoogleMapPlotter(40.71429800,-74.00599700, 10)

# Overlay our datapoints onto the map
gmap.heatmap(latitudes, longitudes)

# Generate the heatmap into an HTML file
gmap.draw("index.html")

# ADD KEY API GOOGLE

s = open("/var/www/html/source/heat/index.html").read()
s = s.replace('<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?libraries=visualization&sensor=true_or_false"></script>', '<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBfkHjpOQgHTgVTs__SvAsbKvairU0-JM4&libraries=visualization&sensor=true_or_false"></script>')
f = open("/var/www/html/source/heat/index.html", 'w')
f.write(s)
```
