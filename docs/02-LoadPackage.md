---
output: html_document
editor_options: 
  chunk_output_type: console
---

# Loading R Packages {#Package2}



The functionalities of some packages may require updated versions of [R](https://www.r-project.org/) and [RStudio](https://www.rstudio.com). To avoid errors, please ensure you are using the most recent releases of R and RStudio, and update your R packages.


```r
update.packages()                         
```

## Installing naturecounts {#Package2.1}

You can install naturecounts from GitHub with the remotes package:


```r
install.packages("remotes")
remotes::install_github("BirdsCanada/naturecounts")
```

After installation, you need to load the package each time you open a new R session.


```r
library(naturecounts)
```

## Additional packages {#Package2.2}

Throughout the book we use the [tidyverse](https://www.tidyverse.org/) R package, which is actually a collection of R packages for data science, including tidyr, dplyr, and ggplot2, among others. The tidyverse package can be installed from CRAN and loaded into R as follows:


```r
install.packages("tidyverse") 
library(tidyverse) 
```

You may find additional packages are needed to manipulate and visualize your data. For example, if you are interested in [Mapping Observation](https://birdstudiescanada.github.io/naturecounts/articles/articles/mapping-observations.html) a suite of packages need to be installed and loaded. The process is similar to that previously describe.
