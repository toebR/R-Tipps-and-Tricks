**Basemap in ggmaps without google API**

Hi there, 

this is a very short introduction on how to gather a basemap for your geodata without being dependent on a google API.
I'm pretty sure that there is a similar instruction somewhere to be found in a blogpost but I did not find it anymore. So I decided to post a simple tutorial on the solution that worked for me.

First load (and perhaps install) the necessary libraries.

    library(sf)
    library(tidyverse)
    library(ggmap)

After setting your working directory etc. you can read in geodata. In this example I use a simple shapefile:

    shape <- st_read(dsn = "shape.shp")
    
When reading in the shapefile, the console output will show if your shapefile has a set crs or if it is not defined yet.
In case it is not defined, **set the crs to the coordinate system that your data actual has.** You can see that by looking at the format of your geometry attribute of the shape. For WGS84 coordinates this would look like this:

    st_crs(shape) <- 4326 #4326 is the ESPG code of WGS84
    
If the crs of your shape is not in WGS84, you can set it by transforming the crs:

    shape <- st_transform(shape, 4326)
    
Now we have our shape in the crs we want.

To create a bbox (extent) for our basemap, we can extract the coordinates from the shape the following way:

    coords <- data.frame(st_coordinates(shape)

We use the retrieved coordinates to specify the bbox around the shape geometry + 10%:
    
    height <- max(coords$Y) - min(coords$Y)
    width <- max(coords$X) - min(coords$X)

    borders <- c(bottom  = min(coords$Y)  - 0.1 * height, 
              top     = max(coords$Y)  + 0.1 * height,
              left    = min(coords$X) - 0.1 * width,
              right   = max(coords$X) + 0.1 * width)

With the "borders" variable, we can now retrieve the stamen map.

     map2 <- get_stamenmap(borders, zoom = 15)
     
to plot the shape on the map we can just use ggmap:

    plot <- ggmap(map2) + 
    geom_sf(data = shape,aes(color = shape$var),inherit.aes = FALSE) +
    plot  
