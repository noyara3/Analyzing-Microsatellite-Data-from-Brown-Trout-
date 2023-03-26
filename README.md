# Analyzing Microsatellite Data from Brown Trout: 
`rgb(9, 105, 218)`	

A Step-by-Step Guide Introduction Microsatellites are highly variable molecular markers that have been widely used to study genetic diversity, population structure, and relatedness in various organisms, including fish. 

I will explore how to analyze microsatellite data from brown trout using R.

## Prerequisites 
Before I begin, make sure you have the following R packages installed: 

## Step 1: Install the Required Packages:

First, I need to install the required packages that I will be using for the analysis. 
In R, I can do this using the *install.packages() function. 

I will be using the following packages: 
ade4: a package for multivariate data analysis 
adegenet: a package for population genetics analysis 
ggplot2: a package for creating graphics, and others..

``` install.packages(c("ggplot2", "adegenet", "dplyr", 'poppr', 'hierfstat', 'reshape2', 'scales', 'RColorBrewer')) ```

 ``` lapply (c("ggplot2", "adegenet", "dplyr", 'poppr', 'hierfstat', 'reshape2', 'scales', 'RColorBrewer'), require, character.only = TRUE) ```


## Step 2: Import Microsatellite Data:
 
I will start by importing the microsatellite data. I’ll assume that the data is in GenePop format. To import the data, use the following R command:

``` Brown.Trout <- read.genepop('file_location/file_name.gen', ncode = 3L) ```

## Step 3: Load the Data 

The data I will be using is a subset of the brown trout dataset from the adegenet package. It contains genetic data for 524 Brown Trout individuals from 5 populations in Finland.
 
To load the data, I will use the read.genepop() function from the adegenet package. 

## Step 4: Check the Data 

Let's check the imported data to ensure it was read in correctly. 
Use the following command to view the imported data: 
```print(Brown.Trout)```

## Step 5: Check for Unique Genotypes

Check if the genotypes are unique using the following command:
```mlg(Brown.Trout)```

## Step 6: Check the Sample Size for Each Site

Check the sample size for each site using the following command:
```summary(Brown.Trout$pop)```

## Step 7: Check Number of Alleles per Locus

Check the number of alleles per locus using the following command:
```table(Brown.Trout$loc.fac)```

## Step 8: Check for Private Alleles per Site

Check the number of private alleles per site across all loci using the following command:
```private_alleles(Brown.Trout) %>% apply(MARGIN = 1, FUN = sum)```

## Step 9: Check Mean Allelic Richness per Site

Check the mean allelic richness per site across all loci using the following command:
```allelic.richness(genind2hierfstat(Brown.Trout))$Ar %>% apply(MARGIN = 2, FUN = mean) %>% round(digits = 3)```

## Step 10: Check Heterozygosity per Site

Check the heterozygosity per site using the following command:
```basic_b_trout = basic.stats(Brown.Trout, diploid = TRUE) Ho_b_trout = apply(basic_b_trout$Ho, MARGIN = 2, FUN = mean, na.rm = TRUE) %>% round(digits = 2) He_b_trout = apply(basic_b_trout$Hs, MARGIN = 2, FUN = mean, na.rm = TRUE) %>% round(digits = 2) Het_Btrout_df = data.frame(Site = names(Ho_Btrout), Ho = Ho_Btrout, He = He_Btrout) %>% melt(id.vars = "Site")```


## Step 11: Calculate heterozygosity per site

(a higher proportion of heterozygous individuals at a particular locus can indicate greater genetic diversity).

```Calculate basic stats using hierfstat > basic_b_trout = basic.stats(Brown.Trout, diploid = TRUE) Ho_b_trout = apply(basic_b_trout$Ho, MARGIN = 2, FUN = mean, na.rm = TRUE) %>% round(digits = 2) Ho_b_trout```

## Step 12: Calculate the mean expected heterozygosity per site.

```He_b_trout = apply(basic_b_trout$Hs, MARGIN = 2, FUN = mean, na.rm = TRUE) %>% round(digits = 2) He_b_trout```

## Step 13: Visualize heterozygosity per site 

(the observed heterozygosity (Ho) and the expected heterozygosity (He)).

```(Het_Btrout_df = data.frame(Site = names(Ho_b_trout), Ho = Ho_b_trout, He = He_b_trout) %>% melt(id.vars = "Site") custom_theme = theme( axis.text.x = element_text(size = 10, angle = 90, vjust = 0.5, face = "bold"), axis.text.y = element_text(size = 10), axis.title.y = element_text(size = 12), axis.title.x = element_blank(), axis.line.y = element_line(size = 0.5), legend.title = element_blank(), legend.text = element_text(size = 12), panel.grid = element_blank(), panel.background = element_blank(), plot.title = element_text(hjust = 0.5, size = 15, face="bold") ) hetlab.o = expression(italic("H")[o]) hetlab.e = expression(italic("H")[e]) ggplot(data = Het_Btrout_df, aes(x = Site, y = value, fill = variable))+ geom_bar(stat = "identity", position = "dodge", colour = "black")+ scale_y_continuous(expand = c(0,0), limits = c(0,0.750), breaks = c(0, 0.10, 0.20, 0.30, 0.40, 0.50, 0.60))+ scale_fill_manual(values = c("pink", "#bdbdbd"), labels = c(hetlab.o, hetlab.e))+ ylab("Heterozygosity")+ ggtitle("B.trout Heterozygosity")+ custom_theme)```

## Step 14: Perform Principal Component Analysis (PCA) 

I will perform a PCA on the dataset using the dudi.pca() function from the ade4 package. 

## Step 15: Plot the PCA Results 

I will plot the results of the PCA using the scatter() function from the adegenet package. 

## Step 16: Perform Discriminant Analysis of Principal Components (DAPC) 

I will perform a DAPC on the dataset using the dapc() function from the adegenet package. This will allow us to identify the number of clusters in the dataset and assign individuals to those clusters. 

## Step 17: Plot the DAPC Results 

I will plot the results of the DAPC using the scatter() function from the adegenet package. This will create a scatter plot of the first two discriminant functions, with individuals colored by assigned cluster

## Step 18: Calculate the inbreeding coefficient (FIS) 
(The FIS value is a measure of the proportion of homozygosity in a population, with higher values indicating a higher level of homozygosity and lower values indicating a lower level of homozygosity).

```FIS_b_trout = apply(basic_b_trout$F, MARGIN = 2, FUN = mean, na.rm = TRUE) %>% round(digits = 2) FIS_b_trout```


## Summary:

We went through the steps involved in conducting a principal component analysis (PCA) and a discriminant analysis of principal components (DAPC) on genetic data for brown trout populations. I started by importing the data and checking for missing values and outliers. I then performed a PCA to identify the most informative genetic variation. I used the output of the PCA to run a DAPC, which allowed us to assign individuals to populations and visualize the genetic structure of the populations. 



