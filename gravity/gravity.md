---
layout: page
title: Gravity Model of Spatial Interaction
---

**Tldr: [the final product!](assets/index.html)**

**Purpose**

A [gravity model](assets/GravityModel.model3) of spatial interaction was created for the purpose of
identifying the potential interaction between two places. The model uses
three variables to calculate potential interaction: the weight/attractiveness
of the input, the weight/attractiveness of the target and the distance between
the origin and the target. These three parameters combine to form the
equation: (inputWeight)^λ * (targetWeight)^α / (distance)^β as referenced
in Rodrigues [The Geography of Transport Systems](https://transportgeography.org/contents/methods/spatial-interactions-gravity-model/)

A case study of hospital service areas in New England was used to
investigate the accuracy of the model, as compared to the hospital service areas
determined by the [Dartmouth Atlas of Health Care](https://www.dartmouthatlas.org/).

**Workflows**

![HSA Pre-Processing Model Workflow](assets/PreProcessModel.png)

[Hospital Data](https://hifld-geoplatform.opendata.arcgis.com/datasets/6ac5e325468c4cb9b905f1728d6fbf0f_0)
from the Homeland Security Administration was pre-processed in a [model](assets/HSAPreProcess.model3)
that removed closed hospitals and hospitals with no beds, as well as filtered out
hospitals that weren't classified as general acute care.

![Gravity Model Workflow](assets/GravityModel.png)
**Gravity Model Workflow**
Additional Details: 
* Distance Matrix allows for the additional input of k, a variable which determines the nearest number of k input features to compare to target features
* Potential was calculated using the equation (inputWeight)^λ * (targetWeight)^α / (distance)^β
* Spatial catchments were calculated by aggregating the sum of the input weights into the target features.

After pre-processing the HSA Hospital Data was put into the gravity model as the
target layer, with beds as the weight field. Town and population data from
the American Community Survey 2018 5 year average was the input layer, with
population as the weight field. The ACS [data](https://gis4dev.github.io/lessons/assets/netown.gpkg)
was put together by Joe Holler.

**You can find the comparison between my gravity model of spatial interaction for
hospital service areas in New England and the Dartmouth Atlas of Health care
[here](assets/index.html).**

Although there are some similarities between the output of my model and the
Dartmouth Atlas of Health Care's geographic boundaries, there is still a lot of
room for the accuracy of my model to be improved. However, both models find areas with limited access to healthcare, in the more rural areas of northern Maine and northern New York, showing a potentially alarming pattern of a lack of access to hospitals in rural areas. In the future, it would be interesting to see if this pattern of lack of access to hospitals in rural areas extends to other parts of the United States, or if we see similar patterns in other countries. Especially during the COVID-19 pandemic, doing a spatial interaction analysis on the location of vaccination sites would also provide an insight into communities that are currently falling below the radar but should be provided additional support, whether that be in aiding transportation to vaccination sites, or changing the locations of vaccination sites themselves. 

**Summary of data sources:**
* [HSA Hospital Data](https://hifld-geoplatform.opendata.arcgis.com/datasets/6ac5e325468c4cb9b905f1728d6fbf0f_0)
* [ACS data](https://gis4dev.github.io/lessons/assets/netown.gpkg)
* [Dartmouth Atlas of Health Care Hospital Service Areas](https://atlasdata.dartmouth.edu/downloads/supplemental#boundaries)

**Complete Models:**
* [Gravity Model of Spatial Interaction](assets/GravityModel.model3)
* [HSA Hospitals Pre-Processing Model](assets/HSAPreProcess.model3)
