---
title: "Pollution and Real Estate"
date: 2019-07-26T22:53:58+05:30
draft: false
# github_link: "https://github.com/gurusabarish/hugo-profile"
author: "Federico Mutasci"
tags:
  - Environmental Economics
  - Real Estate
  - Spatial Econometrics
  - Geospatial Analysis 
image: /ilva.jpg
description: ""
toc: true
mathjax: true

---
This is a summary of my first empirical work, my Master's thesis in applied economics: Mutasci, F. (2019). "The Impact of Pollution on Property Price Evaluation: A Spatial Approach to Hedonic Pricing Models", University of Milan.

## Abstract
The purpose of the study is to investigate one of the problems affecting the economy of the city of Taranto, namely the possibility that the deposit of polluting particulate matter (PM10) related to the presence of the former ILVA\ArcelorMittal steelworks exacerbates the decline in property values in working-class neighborhoods. The study uses a set of geolocated data on houses for sale with corresponding hedonic characteristics, including pollution levels estimated using IDW interpolation of ARPA weather station samples, controlling for socioeconomic variables. The research design enriches classic hedonic pricing models through the use of spatial lag matrices, estimating SAR and SEM models with the maximum likelihood method, showing that, although higher concentration of pullutant particles predicts a drop in property price, the devaluation of houses for sale appears to be mainly confined to the suburban neighborhood called “Tamburi”, located next to the steel mill.

## Project Aims and Relevance
The methods adopted are part of the rich economic literature on Hedonic Pricing Models (Rosen, 1974). The models estimated also include controls that are not strictly related to the endogenous characteristics of houses – such as the socioeconomic conditions in which real estate properties are located, environmental characteristics and pollution levels – that are treated as determinants of property values, following a methodological framework that is not new in the literature (Henning & Ridker, 1969; Yusufa & Resosudarmo, 2009). Moreover, the research design makes use of SAR and SEM Spatial Models (Anselin, 1999; Martin, 1974; LeSage, 2008; Anselin, 1988; LeSage, 2004) to account for spatial autocorrelation.

The topic of the research in the master’s thesis is part of a heated debate on the problematic relationship between the city of Taranto and the big adjacent steel district (Francesca, et al., 2012; ANSA, 2022; V.N., 2019; PeaceLink, 2007), although only focusing on real estate. Although it is a powerful tool, spatial analyses has never been adopted before to highlight how the most polluted neighborhoods and/or those that are close to the exILVA/ArcelorMittal steelworks also seem to be affected by a major problem of property price depreciation. This analytical framework could certainly be readapted to study the real estate market in other Italian and European cities adjacent to highly polluting factories or industrial complexes – in fact, in Italy alone there should be about 60 other factories interested by critical public debate similar to what happens with the ex-ILVA steel mill in Taranto (EEA, 2011). 

## Project Description 
### Data Collection and Compilation of the Dataset
The study makes use of a cross-sectional dataset of houses for sale in the city of Taranto and the neighboring municipality of Statte (which was part of the municipal territory until June 1992), with all observations being geolocated and provided with a set of their characteristics (location, size, number of rooms, energy efficiency class etc.). The working dataset was compiled by:

a) Estimating the concentration of PM10 polluting particles in the air (\\(P_{xy}\\), mostly caused by industrial activity) over the area of analysis, performing an Inverse Distance Weighted (IDW) Interpolation (Shepard, 1968; Mesnard, 2011; Gimond, 2022) of pollution levels recorded at ARPA monitoring stations (ARPA Puglia; ASL Puglia; AReSS Puglia, 2021):

$$
P_{xy} = \frac{\sum_{i=1}^n w_i p_i}{\sum_{i=1}^n w_i}
$$

\\(𝑥, 𝑦\\) being longitude and latitude of each point \\(𝑖\\), \\(𝑤_𝑖 = |𝑥𝑦_{−𝑖} − 𝑥𝑦_𝑖|^{−𝛼} \\) being the inverse of the distance, weighted for the inverse distance power \\(𝛼 \geq 0  \\) (here \\(𝛼 = 2\\) as QGIS default settings). The recorded amount of PM10 polluting particles is taken as the mean annual averages from 2016 to 2019, before and after the sale of the steel plant from the previous management of Riva Group to ArcelorMittal. This method of handling the data on pollution also allows to rule out the year-specific biases in the measurements due to particular weather conditions, such as Sahara sandstorms.

b) Computing a sub-local measure of the percentage of residents with a university degree making use 
of ISTAT census datasets, to be used as an proxy of the socioeconomic conditions of 
neighborhoods. 

c) Collecting a random sample of 428 houses for sale in Taranto with respective characteristics (to be used as hedonic price prectors) from sales advertisements on the websites of the real estate 
agencies.

Each house was geolocated with a pair of coordinates derived from the address mentioned in sales advertisements. The location of the houses was used to compute the distance of each observation from the city center and from the steel mill (also treated as determinants of selling prices), and hence to match each house with (I) the estimated PM10 levels in the location, (II) the percentage of residents with a university degree in the area of the building.

<figure style="text-align:center;">
  <img src="/sample.png" alt="Sample" width="500px"/>
  <figcaption>Figure 1: Data elaboration from QGIS. Observations plotted on a map of the municipalities of Taranto and Statte. </figcaption>
</figure>


### Preliminary Analyses
The existence of some kind of nexus between industrial-related pollution/proximity to the industrial district and real estate is roughly identified through various data visualization tools. A first correlation emerges in the scatterplots of Figure 2 between the logarithm of house prices and:  
a) The the distance of the houses from the former ILVA (taken as a logarithm to capture 
nonlinearity) 
b) Average estimated levels of PM10 polluting particles in the position of each one of the houses for sale of the working dataset

The point clouds in Figure 2 seem to descriptively highlight two clusters of distributions. There is a split between the distribution of the prices of houses for sale at high PM10 levels (set as above 23 μg/m3, still below the legal limit threshold of 50 μg/m3) – with prices falling with higher levels of pollution – and that of houses for sale interested by lower average pollution levels. A second, considerably more noticeable family of clusters appears to involve a split between houses that are within 2.7 kilometers - i.e., within the distance where the Tamburi neighborhood is located- from the ex-ILVA steel mill (with lower average prices) and all others. Creating dummy variable that divides the houses located in the Tamburi district from those in the rest of the city, it is in fact possible to show in the Boxplots two distinct price bands, with houses for sale in the Tamburi neighborhood having both lower average prices and a lower price range.

<figure style="text-align:center;">
  <img src="/scatter.png" alt="Sample" width="500px"/>
  <figcaption>Figure 2:  Data elaboration. From left to right: Scatterplots and Boxplots.  </figcaption>
</figure>

The idea that the Tamburi district represents a cluster of particularly depreciated real estate properties is then further explored through preliminary spatial analyses. Starting from the geospatial position of the houses in the dataset, the area under analysis was divided into Thiessen polygons with each of the observations as centroids, through a Voronoi tessellation (Burrough, McDonnell, & Lloyd, 2008; Longley, Goodchild, Maguire, & Rhind, 2005; Zekai, 2016; Jonathan, s.d.). The polygons obtained were then used to build a Contiguity Weight Matrix (𝑊) (Martin, 1974), defining the neighborhood set of each observation/polygon as the set of all of those observations/polygons with which it shares a side or an edge (Queen contiguity criterion) (Anselin, 2020; Ignacio, 2016).  

To check for spatial autocorrelation between the data in the dependent variable, a Moran Scatterplot (Mendez, 2020) is plotted in Figure 3 using the previously defined adjacency matrix, showing a correlation between the prices per square meter of the houses (accounting therefore for the hidden variable of “house size” in the univariate correlation) and the mean squared price in their neighborhood set. The statistically significant spatial clusters and spatial outliers are thus plotted on a map of the LISA Clusters (ibid.) in Figure 3, revealing that the Tamburi district consists of a low-high type spatial outlier – i.e., the areas immediately adjacent to the steel mill are interested by lower-than-average prices per square meter, despite having houses with higher-than-average prices in their proximity set. Interestingly, the city center (from which both the Tamburi district and the steel mill are visible to the naked eye) constitutes a high-high type spatial cluster, highlighting a marked divide from the Tamburi district. 

<figure style="text-align:center;">
  <img src="/plots.png" alt="Sample" width="800px"/>
  <figcaption>Figure 3:Point Data, underlying the position of the steel plant and the city center, plot of Thiessen Polygons and Queen Contiguity Matrix, Moran scatterplot of prices/m2 and LISA Clusters map. 
 </figcaption>
</figure>

### Spatial Regressions

The evidence gathered in preliminary analyses is then tested in hedonic pricing regression models that account for the existence of spatial spillovers, to avoid spatial biases in the resulting estimates. The hypothesis that a higher concentration of pollutant particles in the air and a major proximity to the steel plant leads to a reduction in the expected prices of houses for sale is tested, all other characteristics of the properties for sale being equal, using PM10 levels and the location of the houses in Tamburi district as hedonic determinants of prices in a series of SAR and SEM models – SAR models 1.1-1.3 and SEM models 2.1-2.3, estimated by Maximum Likelihood. The sub-locale percentage of residents with a university degree is included as an additional predictor to control for the socioeconomic conditions in the location where each of the observations is placed. 

Spatial regression models are built using the spatial weight matrix 𝑊, which can be modelled in the regression equation in two different ways (Anselin, 1988), to obtain: 

#### SAR – Spatial Autoregressive Models
#### SEM – Spatial Error Models 