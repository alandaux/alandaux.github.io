---
layout: page
title: RP- Vulnerability modeling for sub-Saharan Africa
---


**Reproduction of**
# Vulnerability modeling for sub-Saharan Africa

Original study *by* Malcomb, D. W., E. A. Weaver, and A. R. Krakowka. 2014. Vulnerability modeling for sub-Saharan Africa: An operationalized approach in Malawi. *Applied Geography* 48:17–30. DOI:[10.1016/j.apgeog.2014.01.004](https://doi.org/10.1016/j.apgeog.2014.01.004)

Reproduction Authors:
Arielle Landau, Joseph Holler, Kufre Udoh, Open Source GIScience students of fall 2019 and Spring 2021

Reproduction Materials Available at: [alandaux-RP-Malcomb](https://github.com/alandaux/RP-Malcomb)

Created: `14 April 2021`
Revised: `26 April 2021`

## Abstract

The original study is a multi-criteria analysis of vulnerability to Climate Change in Malawi, and is one of the earliest sub-national geographic models of climate change vulnerability for an African country. The study aims to be replicable, and had 40 citations in Google Scholar as of April 8, 2021.

## Original Study Information

The study region is the country of Malawi. The spatial support of input data includes DHS survey points, Traditional Authority boundaries, and raster grids of flood risk (0.833 degree resolution) and drought exposure (0.416 degree resolution).

The original study was published without data or code, but has detailed narrative description of the methodology. The methods used are feasible for undergraduate students to implement following completion of one introductory GIS course. The study states that its data is available for reproduction in 23 African countries.


### Data Description and Variables

Adaptive capacity was calculated using data from DHS. Each household received a scaled value between 1 to 5 for each asset through a percent rank, before the assets were weighted (based on a percent value representing its relative importance for adaptation). After calculating capacity by summing all the weighted capacity fields, the mean capacity for each household was summarized to traditional authorities.

The following were considered as assets for adaptive capacity: number of livestock units, amount of arable land, number of household sick in the past 12 months, wealth index score, number of orphans in the household, time to water source, electricity, type of cooking fuel, sex of head of household, ownership of a cell phone, ownership of a radio and house setting (rural or urban).

Number of livestock units (weighted to 4%) were calculated by summing together 4 fields that counted different types of livestock (cattle, goats, sheep and poultry). Arable land (weighted to 6%) corresponded to one DHS data field that reported the hectares of arable land per household. The number of household sick in the past 12 months (weighted to 3%) was calculated using one field that counted the number of members in a household that were sick for 3+ months in the last year. The number of orphans in a household (weighted to 3%) corresponded to one field that counted the number of orphans and vulnerable children per household. The wealth index score (4%) was one field with a wealth index factor score from 5 classes: poorest, poorer, middle, richer and richest. Time to water source (weighted to 4%) was broken into two choices: water on premises or not. Whether a household had electricity (weighted to 3%), a cell phone (weighted to 4%) or a radio (weighted to 3%) were all evaluated as yes/no binaries. Type of cooking fuel (weighted as 2%) was ranked from electricity, LPG/Natural gas, natural gas, biogas, kerosene, coal/lignite, charcoal, wood, straw/shrubs/grass, agricultural crop, animal dung to no food cooked. Sex of head of household (weighted to 2%) was a male/female binary, with female head of households ranking lower for adaptive capacity because of literature which suggested that females are more vulnerable based on less access to sources of power, land and resources.

Livelihood sensitivity was based on data from the famine early warning network's livelihood zones data, but only for the poor wealth group. Livelihood zones were created based on where households shared similar options for obtaining food and income. Food from their own farm (weighted to 6%) was the percent of food that poor households received independently from their own farms. Income from wage labor (6%) was the percent of income that poor households received from wage labor. Income from cash crops (4%) was the percent of income from selling crops. Disaster coping strategy (4%) was described in the original study (Malcomb. et al.) to be the amount of "ecological destruction associated with livelihood coping strategies during time of crisis in each zone." It was very unclear from this description how disaster coping strategy was actually calculated. We made the decision to calculate it as the percent of income from ecologically destructive activities, which we further determined to be selling firewood, wild foods or grasses.

Physical exposure was calculated using UNEP flood hazard and drought exposure data, collected at the global level. The estimated risk for flood hazard (weighted to 20%) was from an estimate of global risk of flood hazard on a scale of 1 to 5 (extreme). Exposure to drought events (weighted to 20%) was an estimate of drought exposure based on the Standardized Precipitation Index, where the unit is the expected average annual population exposed. Bilinear resampling was used for drought exposure to average the continuous population exposure values. Nearest neighbor resampling was used for flood risk to preserver integer values The drought raster was also reclassified into quantiles.

For livelihood sensitivity and adaptive capacity, all fields used were percent ranked on a scale 1 - 5, before being weighted as detailed above. The calculated livelihood sensitivity and adaptive capacity results were then rasterized in order to calculate vulnerability with physical exposure.

### Analytical Specification

The original study was conducted using ArcGIS and STATA, but does not state which versions of these software were used.
The reproduction study will use R.

## Materials and Procedure

**Workflow Version 1:**
#### Data
2004 - 2010 DHS w/ GPS
Demographic and health survey (assets & access)
UNEP/grid Europe (biophysical exposure)
Famine early warning network (livelihood)

#### Preprocessing of Geographic Boundaries (Step 1)
2004-2010 DHS data points (for each village surveyed): District → ***disaggregated*** →
villages → ***disaggregated*** → traditional authorities

#### Step 2
Data Input: UNEP/grid Europe, Famine early warning network → ***Raster*** → ***Weight values***: All vulnerability measures were weighted (table 2) and normalized between 0 & 5 (RStudio)

#### Step 3: Creating the Model of Vulnerability
***Calculate***: Household resilience = adaptive capacity + livelihood sensitivity - biophysical exposure

**Workflow Version 2**
1. Data Preprocessing:
    1. Download traditional authorities: MWI_adm2.shp
2. Adding TA and LZ ids to DHS clusters
3. Removing HH entries with invalid or unknown values
4. Aggregating HH data to DHA clusters, and then joining to traditional authorities to get: ta_capacity_2010
5. Removing index and livestock values that were NA
6. Sum of Livestock by HH
7. Scale adaptive capacity fields (from DHS data) on scale of 1 - 5 to match Malcomb et al.
8. Weight capacity based on table 2 in Malcomb et al.
9. Calculate capacity by summing all weighted capacity fields
10. Joining mean capacities to TA polygon layer
    1. Summarize capacity from households to traditional authorities
11. Making capacity score resemble Malcomb et al's work (scores on range of 0-20) by multiplying capacity score by 20
12. Categorizing capacities using natural jenks methods
13. Creating blank raster and setting extent of Malawi - CRS: 4326
14. Reproject, clip and resampling flood risk and drought exposure rasters to new extent and cell size
    1. Uses bilinear resampling for drought to average continuous population exposure values
    2. Uses nearest neighbor resampling for flood risk to preserve integer values
    3. Removing factors and recasting them as integers
    4. Clipping TAs with LZs to remove lake
15. Rasterizing final TA capacity layer
16. Masking flood and drought layers
17. Reclassify drought raster into quantiles
18. Add all RASTERs together to calculate final output:  final = (40 - geo) * 0.40 + drought * 0.20 + flood * 0.20
19. Use zonal statistics to aggregate raster to TA geometry for final calculation of vulnerability in each traditional authority

**Workflow Version 3**:
-add in livelihood zones procedures, also comparing our maps to the ones in malcomb et al.
1. Data Preprocessing:
    1. Download traditional authorities: MWI_adm2.shp
2. Adding TA and LZ ids to DHS clusters
3. Removing HH entries with invalid or unknown values
4. Aggregating HH data to DHA clusters, and then joining to traditional authorities to get: ta_capacity_2010
5. Removing index and livestock values that were NA
6. Sum of Livestock by HH
7. Scale adaptive capacity fields (from DHS data) on scale of 1 - 5 to match Malcomb et al.
8. Weight capacity based on table 2 in Malcomb et al.
9. Calculate capacity by summing all weighted capacity fields
10. Joining mean capacities to TA polygon layer
    1. Summarize capacity from households to traditional authorities
11. Making capacity score resemble Malcomb et al's work (scores on range of 0-20) by multiplying capacity score by 20
12. Categorizing capacities using natural jenks methods
13. Calculate livelihood sensitivity:
    1. Pre- process livelihood data from famine early warning network livelihood zones
        1. Disaster coping strategy: % income from selling firewood, wild foods or grasses (income from firewood + income from grasses + income from wild foods / total income * 100)
        2. %Income from Cash Crops: (cash from crops / total income * 100)
        3. %Food from own farm: (% of crops reported as sources of food)
        4. %Income from wage labor: (labour etc. as source of cash / total income * 100)
    2. Scale livelihood sensitivity fields on scale of 1 - 5 to match Malcomb et al.
    3. Weight capacity based on table 2 in Malcomb et al.
    4. Calculate livelihood sensitivity by summing all weighted fields
14. Creating blank raster and setting extent of Malawi - CRS: 4326
15. Reproject, clip and resampling flood risk and drought exposure rasters to new extent and cell size
    1. Uses bilinear resampling for drought to average continuous population exposure values
    2. Uses nearest neighbor resampling for flood risk to preserve integer values
    3. Removing factors and recasting them as integers
    4. Clipping TAs with LZs to remove lake
16. Rasterizing final TA capacity layer and final livelihood sensitivity layer
17. Masking flood and drought layers
18. Reclassify drought raster into quantiles
19. Add all RASTERs together to calculate final output:  final = (40 - geo) * 0.40 + drought * 0.20 + flood * 0.20 + lhz_sensitivity * 0.20
20. Use zonal statistics to aggregate raster to TA geometry for final calculation of vulnerability in each traditional authority

## reproduction Results

**Adaptive Capacity by Traditional Authority**
[Adaptive Capacity by Traditional Authority](figures/ac_2010.png)

**Difference in Reproduction and Original Study for Adaptive Capacity**
[Adaptive Capacity Difference](figures/ACDiff.png)

**Confusion Matrix for Adaptive Capacity**
[Confusion Matrix](figures/confusionMatrix.png)

The difference between the reproduction of adaptive capacity by traditional authority, and that of the original study was calculated using Spearman's Rho. Spearman's Rho calculates the correlation of ranked data with the assumption that there is no correlation (the null hypothesis). A value of 0 means no correlation, -1 designates an inverse correlation, and 1 designates a positive correlation. The Spearman's Rho for the comparison between our adaptive capacity map and Malcomb's resilience map was 0.7795965, indicating a somewhat strong positive correlation.

**Map of Vulnerability in Malawi**
[Map of Vulnerability in Malawi](figures/vulnerability.png)

A scatterplot comparing raster values between the reproduction and original study vulnerability maps similar indicates a weak correlation.
[Scatterplot](figures/fig5DiffScatterplot.png)


**Difference in Reproduction and Original Study for Vulnerability**
[Vulnerability Difference](figures/VulDiff.png)

The difference between our reproduction of vulnerability in Malawi and that of the original study was similarly calculated using Spearman's Rho. The Spearman's Rho for this comparison was 0.271046, indicating a weak positive correlation between our vulnerability map and Malcomb's.

Overall, the adaptive capacity map was supported by the reproduction, but the vulnerability map was not supported by the reproduction.


## Unplanned Deviations from the Protocol

Based on the Malcomb et al. paper, it is unclear whether Figure 4, which shows resilience scores in traditional authorities is just showing adaptive capacity, or whether it takes into account livelihood sensitivity and physical exposure. The caption of the figure only mentions DHS indicators. Since DHS data is only used in calculating adaptive capacity, we made the decision reproduce Figure 4 as a measure of adaptive capacity in each traditional authority. Additionally, the discussion section of Malcomb et al. uses Figure 4 to interpret adaptive capacity.

It was also unclear how Malcomb et al. calculated disaster coping strategy. Disaster coping strategy (4%) was described in the original study (Malcomb. et al.) to be the amount of "ecological destruction associated with livelihood coping strategies during time of crisis in each zone." We made the decision to calculate it as the percent of income from ecologically destructive activities, which we further determined to be selling firewood or grasses.

Additionally, when subsequently running the R script, the DHS data is already downloaded, making the code blocks which download this data break. Emma Brown, a classmate, figured out that where we called the downloaded DHS data in the R script, we instead needed to provide a direct file path to the already downloaded data.

## Discussion
Our reproduction of Malcomb et al. provided lots of insight into the detail required in the original study in order to produce a successful reproduction. The detail provided in how Malcomb et al. calculated adaptive capacity was sufficient because almost all of the indicators used corresponded directly to data fields in DHS data, leaving less room for error. This was shown in our successful reproduction of Figure 4, the adaptive capacity for each traditional authority, with a Spearman's Rho of 0.7795965. The lack of a perfect reproduction is likely a result of several uncertainties. One of those uncertainties is in how vulnerability indicators were ranked. Throughout Malcomb et al., data is discussed as being broken into quantiles, or on a scale of 0 - 5. While quantile designates 5 categories, a scale of 0 - 5 indicates 6, making it unclear on what scale the data should be re-classified. It is also unclear how Malcomb et al. converted binary categories, such as sex, to quintiles or a scale of 0 - 5. For the purposes of our reproduction, we used percent rank to re-classify data into 5 categories. Another uncertainty was at which level to weight data fields and calculate adaptive capacity. We decided to calculate adaptive capacity at the household level, and then aggregate to traditional authorities, but it is entirely possible that Malcomb et al. calculated adaptive capacity at the traditional authorities level by aggregating the raw household data first.

The lack of detail in Malcomb et al. on how livelihood sensitivity was calculated meant that our reproduction of vulnerability in Malawi was a failure, with a Spearman's Rho of 0.271046. There was much uncertainty in which data fields for Malawi's livelihood zones corresponded to Malcomb et al's categories of %income from wage labor, %income from cash crops and disaster coping strategy. For %income from wage labor, it was unclear whether income from self-employment, in addition to labor should be counted (we decided to just use income from labor). For %income from cash crops, it was unclear what crops were considered to be cash crops, or whether we should count all income from crops (what we ultimately decided). Disaster coping strategy had the most uncertainty, with no guidance on what data fields could possibly correspond to disaster coping strategy (we summed the income from selling firewood, wild foods and grasses, assuming these practices to be ecologically destructive activities). Because this uncertainty was introduced early on in the vulnerability model, the error is likely to multiply as we move through the analysis.

As no code was published in Malcomb et al., the entire workflow is uncertain as there is no clarification in how data was scaled, weighted and at what level any kind of aggregation or calculations occurred. Making the code available for their statistical analysis would go a long way in clarifying decisions that did not seem important to include in a methods section, but become incredibly relevant when trying to produce a successful reproduction.

Our reproduction also sheds light on larger debates in the field of geography on issues of error and uncertainty. Longley (2008) introduces a model for thinking about error and uncertainty in spatial research where error and uncertainty is introduced at each step of spatial analysis. This process is represented in a timeline where error and uncertainty is introduced at every step: real world --> conception --> measurement & representation --> analysis --> interpretation and validation. Every step takes us further and further away from an accurate representation of the real world. The problem with Malcomb et al. is that we do not even know the details of how they went from their data sources to their meta-themes of measuring vulnerability, to how they analyzed data to fit their categories of resilience. We know there is uncertainty inherent in this process, but the lack of detail on their process of spatial analysis means we cannot estimate or discuss what kind of uncertainty was introduced, or how it was introduced. Tate (2013) also provides a helpful framework in thinking about vulnerability model and uncertainty. Tate (2013) outlines decisions in each phase of modeling that could introduce uncertainty in models. However, there are several points in decision making for a model that are completely ignored by Malcomb et al. There is no decision made on how to measure error, and there is also no phase of the model where Malcomb et al. conduct an uncertainty or sensitivity analysis, or validate their findings in any way. There is thus no way for us to estimate how much uncertainty was introduced by Malcomb et al. in their decision. We have no idea how much their choices mattered for the final result.

## Conclusion

Our partial successful reproduction of Malcomb et al. shows the need to clarify data fields used in any kind of calculation, as well as the need to publish code to clarify procedures. In reproduction Malcomb et al., we contributed to theories that reproduction is key in uncovering uncertainties in vulnerability models, and that open source GIScience is an important step in minimizing uncertainty in geographic studies.

## A Big Thank You To All My Group Members!
This was largely a group project, and this reproduction could not have been done without Maddie Tango, Jackson Mumper, Evan Killion, Sanjana Roy and Steven Montilla-Morantes.

## References
Longley, P. A., M. F. Goodchild, D. J. Maguire, and D. W. Rhind. 2008. Geographical information systems and science 2nd ed. Chichester: Wiley. (only chapter 6: Uncertainty, pages 127-153)

Malcomb, D. W., E. A. Weaver, and A. R. Krakowka. 2014. Vulnerability modeling for sub-Saharan Africa: An operationalized approach in Malawi. Applied Geography 48:17–30.

Tate, E. 2013. Uncertainty Analysis for a Social Vulnerability Index. Annals of the Association of American Geographers 103 (3):526–543. doi:10.1080/00045608.2012.700616.

Include any referenced studies or materials in the [AAG Style of author-date referencing](https://www.tandf.co.uk//journals/authors/style/reference/tf_USChicagoB.pdf).

####  Report Template References & License

This template was developed by Peter Kedron and Joseph Holler with funding support from HEGS-2049837. This template is an adaptation of the ReScience Article Template Developed by N.P Rougier, released under a GPL version 3 license and available here: https://github.com/ReScience/template. Copyright © Nicolas Rougier and coauthors. It also draws inspiration from the pre-registration protocol of the Open Science Framework and the reproduction studies of Camerer et al. (2016, 2018). See https://osf.io/pfdyw/ and https://osf.io/bzm54/

Camerer, C. F., A. Dreber, E. Forsell, T.-H. Ho, J. Huber, M. Johannesson, M. Kirchler, J. Almenberg, A. Altmejd, T. Chan, E. Heikensten, F. Holzmeister, T. Imai, S. Isaksson, G. Nave, T. Pfeiffer, M. Razen, and H. Wu. 2016. Evaluating replicability of laboratory experiments in economics. Science 351 (6280):1433–1436. https://www.sciencemag.org/lookup/doi/10.1126/science.aaf0918.

Camerer, C. F., A. Dreber, F. Holzmeister, T.-H. Ho, J. Huber, M. Johannesson, M. Kirchler, G. Nave, B. A. Nosek, T. Pfeiffer, A. Altmejd, N. Buttrick, T. Chan, Y. Chen, E. Forsell, A. Gampa, E. Heikensten, L. Hummer, T. Imai, S. Isaksson, D. Manfredi, J. Rose, E.-J. Wagenmakers, and H. Wu. 2018. Evaluating the replicability of social science experiments in Nature and Science between 2010 and 2015. Nature Human Behaviour 2 (9):637–644. http://www.nature.com/articles/s41562-018-0399-z.
