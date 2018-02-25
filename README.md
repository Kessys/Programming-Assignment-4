# Getting and Cleaning Data Project

1. Download the data source and put into a folder on your local drive. Unzip the file. You'll have a ```UCI HAR Dataset``` folder.
2. Put ```run_analysis.R``` in the parent folder of ```UCI HAR Dataset```, then set it as your working directory using ```setwd()``` function in RStudio.
3. The ```reshape2``` library must be loaded.
4. Run ```source("run_analysis.R")```, then it will generate a new file ```tidy.txt``` in your working directory.
