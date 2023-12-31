# UN-Datathon-2023
## Monitoring Land Use in Biodiversity Hotspots Affected by Forest Loss
![](/sentinel2misiones.png?raw=true "")
### Green Analytics


## Problem Statement
South America's Tri-national Paraná Atlantic Forest, an ecological region shared by Argentina, Brazil, and Paraguay, is one of the world's most diverse ecosystems but is also highly vulnerable to deforestation. Despite significant legislative efforts by all three governments to protect the remaining forests, there is a lack of comprehensive studies evaluating deforestation trends and associated factors in this region. Many of the affected areas are what is usually called "biodiversity hotspost". These are regions that contain a high level of species diversity, many endemic species (species not found anywhere else in the world) and a significant number of threatened or endangered species. The main causes of forest loss are associated with agricultural activities, changes in land use, infrastructure construction, and cattle ranching. 

Deforestation is generated by different factors and it's related to the deficient exercise of the law, and the scarcity of the different controls required in these critical areas. Unfortunately, as a consequence, there is a critical loss of biodiversity, an imbalance of the cycles that govern nature, such as the hydrological cycle, the nutrient cycle, at the same time, the increase of greenhouse gases, contribution to erosion, flooding, salinization of soils. 

The purpose of this project is to monitor land use over time in deforested areas that are key for biodiversity, specifically in the region of Misiones, Argentina, in order to determine the changes from a chronological point of view according to these variables. We consider particularly impoirtanto to understand, not only what areas suffer from deforestation, but what happens with that land afterwards. Is it being restored properly, or is it being transformed into real estate developments or farms? Controlling such large areas can be challenging, so with our approach we propose to apply semantic segmentation algorithms on satellite images to help keep track of the use of these deforested areas.

 Even though we are focusing in a specific area due to time reasons, the methodology can be applied to any areas of interest.


### Misiones
Agricultural activities have been transforming the areas of native forests, which represents a problem of enormous concern in the South American region. In all areas of deforestation there is a common element, which is the change in land use.  In Misiones, through historical occupation, centered on internal and external migrations, the rapid population growth generated a considerable loss of native forests, which was progressive from the plan created since 1975 in the repopulation of Misiones. In this area of South America, national and provincial parks are areas of high value and key to the conservation of natural resources. By 2013, it was reported that in the native forest of Misiones the most commercially valuable species had been extracted without adequate conservation management. Species such as Araucaria angustifolia and Aspidosperma polyneuron are very close to extinction. 

According to Rainforests statistics (2022) comparing for the years 2001-2020, the loss of vegetation cover has increased in Misiones especially in the areas of Libertador General San Martin (with 18.1% representing 6,817 ha of vegetation cover loss), Veinticinco de Mayo (20.1% = 9,168 ha of vegetation cover loss), Eldorado (12.7% = 8,748 ha of vegetation cover loss). In spite of the regulations, which have contributed to reduce deforestation activities in the area. The practices to avoid this problem have not been sufficient. The consequence of the different activities that generate the marked deforestation in Misiones, generates a great concern for the replacement of natural ecosystems, ecosystem services as a source of genetic resources, nutrient supply, habitat, among others. 

As is well known, the Misiones rainforest is a remnant of the Atlantic Forest. It was distributed along the Atlantic coast of Brazil and part of Paraguay. This study area represents an indicator to determine the affectation and impact generated by deforestation as a result of land use change, which is a problem that affects many areas of South America, generating a significant ecosystemic impact, with catastrophic consequences to nature, which is a fact that we cannot see it isolated but integral in the same region that involves several countries. Part of the objective of this project is to be able to zone areas that can be channeled as conservation areas and that are key for biodiversity.


## Methodology
The project was done in two parts, using ArcGIS and Amazon Sagemaker. Due to the reduced time available in the datathon, we performed the outline of the project in ArcGIS and, on the other hand, created a notebook in Sagemaker using its geospatial capabilities as kickstart for future work to apply more accurate algorithms.

### Part 2: Amazon Sagemaker
Amazon Sagemaker geospatial capabilities make it easier to build, train and deplot models using geospatial data. We based our work in the examples present in the AWS github repository ( https://github.com/aws/amazon-sagemaker-examples/tree/main/sagemaker-geospatial ), focusing on the brasil-deforestation-monitoring and mount-shashta-glacier-melting-monitoring.

#### Copernicus and Sentinel
Copernicus is the Earth observation component of the European Union’s Space programme, and the Sentinel-2 mission is a land monitoring constellation of two satellites that provide high resolution optical imagery and provide continuity for the current SPOT and Landsat missions. The mission provides a global coverage of the Earth's land surface every 5 days, making the data of great use in ongoing studies.

For this part of the project, we used the  Sentinel-2 Cloud-Optimized GeoTIFFs  dataset in the Registry of Open Data from AWS.  This dataset is the same as the Sentinel-2 dataset, except the JP2K files were converted into Cloud-Optimized GeoTIFFs (COGs). Additionally, SpatioTemporal Asset Catalog metadata has were in a JSON file alongside the data, and a STAC API called Earth-search is freely available to search the archive. This dataset contains all of the scenes in the original Sentinel-2 Public Dataset and will grow as that does. L2A data are available from April 2017 over wider Europe region and globally since December 2018.

#### Forest health

We start an Earth Observation Job, available with Sagemaker geospatial capabilities, to calculate the NDVI (Normalized Difference Vegetation Index). This index is used to calculate vegetation "greeness" and is useful in understanding vegetation density and assessing changes in plant health. NDVI is calculated as a ratio between the red band (R) and near infrared band (NIR) values in traditional fashion: 

(NIR - R) / (NIR + R)

Both of these bands, along with several more, are available in the Sentinel-2 data so we can easily calculate the index to infer the general state or "health" of the forest.

 The index is a ratio between -1 to +1, where values near -1 have low vegetation reflectance and values near +1 have high vegetation reflectance.  Healthy vegetation will reflect more near infrared and green light while absorbling more red and blue light. 

To obtain this data, we launch an Earth Observation Job in the notebook using the BandMathConfig option to calculate the NDVI.

![](/deforestation.gif?raw=true "")


#### Land Use
After assesing areas affected by deforestation, we can move on to analyzing the land use over time. What we intend is to comprehend what has happened to affected or deforested areas and what has been done with that land over the years.

![Imagen Sentinel Misiones](/misiones-sentinel-2-2.png?raw=true "Imagen Real Sentinel-2 Misiones")

![Land cover mask Misiones](/land-use-misiones.png?raw=true "Land cover mask Misiones")

For that, we launch another Earth Observation Job in Sagemaker changing the configuration to LandCoverSegmentationConfig, that will automatically perform a semantic segmentation algorithm on all the images. Semantic segmentation is a deep learning algorithm that associates a label or category with every pixel in an image. It is used to recognize a collection of pixels that form distinct categories. Information about the available configurations can be found  here .







What we propose as future work, after all of this analysis, is to use a custom neural network to perform semantic segmentation after being fine-tuned on a dataset containing more classes and being similar to the area of interest. It would be particularly important to be able to determine if there has been constructions on those areas (houses of hotels) or farms, to keep track of that land and see if local regulations are being followed.
