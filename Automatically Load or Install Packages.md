**Snippet to load or install & load predefined packages in your R Session**


Hi there, 

In this section, I will provide a short snippet that lets you predefine packages in R which are afterwards loaded into your R Session or installed + loaded if they are not installed yet.
Say you need to load 6 libraries into your R session. I didnt write the snippet but I really want to share it since it really fascilitates my workflow.
Normally we would approach it like that:

```
library(readxl)
library(ggplot2)
library(sf)
library(shiny)
library(tmaptools)
library(leaflet)
```
Now with 6 packages this seems manageable, but what about say 10+ packages?? This approach becomes a burden.
The snippet I use for this case is the following. (1) We store our packages in a string vector e.g.:
```
packages <- c("readxl",
              "ggplot2", 
              "sf",
              "shiny",
              "tmaptools",
              "leaflet"
)
```
Now the snippet that checks the packages and loads them into the R session (if not installed, it prior installs them):
```
package.check <- lapply(
  packages,
  FUN = function(x) {
    if (!require(x, character.only = TRUE)) {
      install.packages(x, dependencies = TRUE)
      library(x, character.only = TRUE)
    }
  }
)
```
