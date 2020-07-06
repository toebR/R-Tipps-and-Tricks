**Workflow to create the same output for multiple files of identical structure (e.g. excel files)**

Hi there, 
This small tutorial is about how I create the same output (e.g. statistical summaries, plots) for similarly shaped input files.
Databases often have default export products such as excel sheets that show the product of a standardized SQL query.
Now what if we want to know how the data of such a query is changes along the exports? (e.g. query that creates an excel sheet everyday about some HR parameters
or excel sheets from semi-automated chemical measurements in a laboratory setting).
This is how I do it:<br/>Load libraries and set working directory to where your excel sheets are stored:

    library(readxl)
    library(tidyverse)
    
    setwd("your working dir")
    
List all xlsx files in the dir you have stored your same-structured xlsx datasheets:

    datsheets <- c(list.files(dir, pattern = "\\.xlsx$"))
    
Create a for loop that uses the datsheets-list as an iterator and create a scatter plot for each of them:

    for (i in datsheets) {
    data = data.frame(read_excel(i))
    ggplot(data) +
    geom_point(aes(x = x, y = y, color = var)) -> plot
    ggsave(plot, filename=paste(i, plot, sep = ""))
    }




