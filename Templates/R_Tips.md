```
r_dataframe <- as.data.frame(nzQuery("SELECT * FROM TEST_TABLE"))
#pulls data directly into memory. r_dataframe can be anything
#as.data.frame is necessary
nz_dataframe$DEMAND <- as.numeric(nz_dataframe$DEMAND)
#changes format, and replaces column. Otherwise leads to separate columns.
#library(dplyr)
# Run sql on sql already loaded into R. Can select specific columns.
#removes columns that confuse R. Specific to dplyr 
corr_mat=cor(newtable,method="s")
newtable<-select(r_dataframe, CPC, DEMAND)
corrplot(corr_mat)
Load the Netezza packages into the RStudio workspace
library(nzr)
library(nza)
library(nzmatrix)
nzConnectDSN('NZSQL')
nzDisconnect()
```
