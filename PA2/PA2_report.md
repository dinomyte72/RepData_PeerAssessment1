---
title: "Exploration of NOAA Storm Data"
author: "Dino Chioda"
date: "May 23, 2015"
output: html_document
---

## Downlaod and load data

```r
url <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
if (!file.exists("/Users/Dino/Documents/git/repro_research/PA1/storm.csv.bz2")) {
    download.file(url, "/Users/Dino/Documents/git/repro_research/PA1/storm.csv.bz2", method="curl")
}
# unzip file
if (!file.exists("/Users/Dino/Documents/git/repro_research/PA1/storm.csv")) {
    library(R.utils)
    bunzip2("/Users/Dino/Documents/git/repro_research/PA1/storm.csv.bz2", "/Users/Dino/Documents/git/repro_research/PA1/storm.csv", remove = FALSE)
}
```

```
## Loading required package: R.oo
## Loading required package: R.methodsS3
## R.methodsS3 v1.7.0 (2015-02-19) successfully loaded. See ?R.methodsS3 for help.
## R.oo v1.19.0 (2015-02-27) successfully loaded. See ?R.oo for help.
## 
## Attaching package: 'R.oo'
## 
## The following objects are masked from 'package:methods':
## 
##     getClasses, getMethods
## 
## The following objects are masked from 'package:base':
## 
##     attach, detach, gc, load, save
## 
## R.utils v2.0.2 (2015-04-27) successfully loaded. See ?R.utils for help.
## 
## Attaching package: 'R.utils'
## 
## The following object is masked from 'package:utils':
## 
##     timestamp
## 
## The following objects are masked from 'package:base':
## 
##     cat, commandArgs, getOption, inherits, isOpen, parse, warnings
```

```r
# load data into R
storm <- read.csv("c:/coursera/storm.csv")
```

```
## Warning in file(file, "rt"): cannot open file 'c:/coursera/storm.csv': No
## such file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```
