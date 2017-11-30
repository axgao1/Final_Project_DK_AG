# The Impact of Distance to a Hospital on Regional Health Outcomes in the US

# The Question

This project explores the impact of distance from a hospital on health outcomes and contrasts the impact in Northern states against that of southern states in the U.S. The northern states we used are Montana, North Dakota, and South Dakota. The southern states we used are Alabama, Louisiana, and Mississippi. Northern states are generally considered to have better health outcomes than southern states. Health outcomes are defined as deaths per 100,000 people. 

The initial hypothesis was that being farther from a hospital has a negative impact on health outcomes, and this negative impact would be larger in southern states than in northern states. After controlling for demographic variables such as age, race, income, sex, education, and unemployment rate, we hypothesized that distance is negatively correlated with health outcomes. Further, we expected distance to have a stronger negative correlation with particular health outcomes that are more sensitive to time/distance to hospitals, such as heart-related diseases relative to health outcomes that are not as sensitive to time/distance, such as Alzheimer's. However, after running our regressions, we do not find any significant correlations between health outcomes and distance. We offer several potential reasons in the later sections.

# The Data [SHOULD WE MAKE CHECKOUT SCRIPT FOR ALL THE DATA?]

To construct our analysis, we downloaded census tract shape files for each state from the Census Bureau, coordinates for hospitals in our 6 states from Medicare's Compare website, health outcomes from CDC Wonder, demographic data from the ACS and CDC Wonder, and a 2016 county FIPS to county name crosswalk from the NBER for the purpose of merging our data. All of our data is from 2016.

Below is a list of the datasets we used.

* [2016 Census tract coordinates](https://www.census.gov/geo/maps-data/data/tiger-line.html)
* [Hospital coordinates](https://data.medicare.gov/Hospital-Compare/Hospital-General-Information/xubh-q36u)
* [2016 Demographic data (education, unemployment, median income) from ACS](https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t)
* [2016 Demographic data (sex, race, age) from CDC](https://wonder.cdc.gov/controller/datarequest/D76)
* [2016 Health outcomes](https://wonder.cdc.gov/controller/datarequest/D76)
* [2016 County crosswalk](http://www.nber.org/cbsa-msa-fips-ssa-county-crosswalk/2016/)

Our analysis occurs at the county level. The distance of a census tract centroid to the closest hospital is averaged across all tracts in a county to approximate a county level distance to hospital metric. All other data is collected at the county level. Education is expressed as percentage of total population within the specific education level. The unemployment rate is that for those over the age of 16. Sex is also expressed as percentages of total county population. Race is presented as percentage of the population that is non-hispanic white. We specified age to be percentage of total county population over the age of 65.

# The Investigation [SHOULD CHANGE MISSING DATA TO WHITE FOR CONTRAST. MAKE LEGEND BIGGER]

All analysis code for this project is included in a single jupyter notebook.

[Health Outcomes and Distance](https://github.com/axgao1/Final_Project_DK_AG/blob/master/Final%20Project.ipynb)

The bulk of the analysis is performed in python, pandas and statsmodels.

We first obtained census tract centroid coordinates for each state using the downloaded shape files. We then converted the centroid coordinates and hospital coordinates to an Alber's Equal Area projection and found the minimum distance from each centroid to a hospital. 

Below is a map of hospital locations with tracts colored by the distance of their centroids to the nearest hospital for the northern states, Montana, North Dakota, South Dakota, and the southern states, Louisiana, Alabama, and Mississippi.

<img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/Hospital%20Locations%20with%20Distances.png?raw=true" width="500" height="500">

We then aggregated this data to the county level and merged it with all of our demographic variables as well as our health outcomes data. Since we also decided to explore how distance impacts change with different causes of death, we brought our demographics and distance measure together with three different measures of mortality: all cause mortality, mortality from various types of heart failure, and mortality from Alzheimer's. Using these data sets, we performed three regreessions detailed below to ascertain the effect of distance to a hospital on our three measures of health outcomes secular to the demographics as well as state fixed effects. 

# Regression Analysis

## Baseline Model - All Cause Mortality Model

Our baseline model uses all causes of mortality (allcausemort) as the dependent variable. We regress log(all cause mortality) on percent of the population with a ged, percent of the population with a high school diploma, percent of the population with some college, percent of the population with an associates degree, percent of the population with a bachelors, percent of the population with a graduate degree or higher, log(median income), percent of the population that is male, percent of the population that is non-hispanic white, percent of the population that is 65+, U3 unemployment, the average minimum distance to a hospital in the county, and state. We hoped to demonstrate that being farther away (a kilometer further) from a hospital increases mortality after having controlled for other variables that would also affect health outcomes.

Our other two regressions had the same independent variables and function form, but used log(heart condition mortality) and log(Alzheimer's mortality) as their dependent variables. We hypothesized finding that mortality from heart conditions would be more sensative to hospital distance than either all cause mortality or mortality from Alzheimer's. 

Given that the effect of distance in our baseline results (all causes of mortality), heart-related, and Alzheimer's-related mortality results were all not significant, we did not pursue models that interacted region fixed effects with distance. These models would have shown the effect of distance from hospitals in each of the two regions.

# The Results

Overall, we hoped to demonstrate that being farther from the hospital has a negative impact on health outcomes and that this impact is larger for heart-related deaths, particularly in the Southern states.

In our first regression, we examined the effect of being further from a hospital on all causes of mortality. Below are our regression results.

In examining our results, we see that the mindist coefficient (-2.456e-05) is not only insignigificant, but the sign on the coefficient is not what we expected either. Being a kilometer further from the nearest hospital should not decrease mortality in a county. 

## ALL CAUSE MORTALITY REGRESSION


| Dep. Variable: | np.log(allcausemort) | R-squared:	0.610 |
---------------- | -------------------- | ----------------- |
OLS              | Adj. R-squared:      | 0.592
Content Cell     | Content Cell         |



This counter intuitive result is also borne out in the heart-related mortality regression. The coefficient on mindist in this regression is -0.0012, although this is still insigificant. This is especially surprising because we expected heart-related mortality to be extremely sensitive to distance from a hospital. When a person is having a heart attack, we would expect distance and time to a hospital to be critical for survival rate. In our results, we only explain 36.2% of the variation in heart-related mortality, so our model is missing key factors.

## HEART REGRESSION

[TABLE]


In examining the Alzheimer's-related mortality regression, the coefficient on mindist (0.0016) is positive as expected, although this is still insignificant. We do expect that being a kilometer further from the nearest hospital is correlated with higher mortality due to Alzheimer's. Ironically, the measure we expect to be the least sensitive to distance from hospital seems to be the most sensitive with a correct correlation direction and the highest t-statistic of the three coefficients.

## AL REGRESSION

[AL TABLE]

## Analysis of covariates
In examining the state fixed effects, it appears that the differences between states are not as large as we expected. No state's mortality outcomes are significantly different from Alabama, except for Montana in the Alzheimer's-related mortality regression, where Montana has significantly lower Alzheimer's-related mortality than Alabama. Though insignificant, North Dakota and South Dakota generally tend to have higher mortality than Alabama. All of this contradicts our initial assumption that Northern states have better health outcomes than Southern states. However, we did not rigorously test the difference in health outcomes between Northern and Southern states, a potential limitation to our study commented on below.

Reviewing the rest of our control variables, we see that age is the biggest predictor in causes of mortality, especially since we limited it to population above 65. Having more old people leads to more deaths. 

# Limitations of our Study and Possible Extensions 

## Missing data

The first limitation to our study is the fact that, though we had complete data for all cause mortality, our data by cause of mortality is highly fragmentary. This can be easily seen in our maps below showing data availability. There is a lot of missing data in the Northern states especially. Without this data, our effective number of observations is extremely limited as counties with missing mortality rates are excluded from our regression results, making our results inaccurate. We tend to be missing more data in rural areas far from hospitals, which compounds the problem. An extension of this study would need to use more complete data for mortality by specific causes.

[WILL ADD 3 DATA MAPS]

<center><img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/All%20Cause%20Data%20Availability%20Map.png?raw=true" width="500" height="500" align="middle"></center>

## Hospital heterogeneity
We also did not control for differences between hospitals and treated hospitals as homogeneous entities. In reality, different hospitals provide vastly different quality of service and many lack particular capabilities all together. An extension of this study would attempt to more richly model what hospitals can and cannot do as well as the quality of service they provide. 

## Additional demographic controls

Additionally, we should have controlled for other demographic variables that may be correlated both with distance from a hospital and health outcomes. For example, rural populations likely face different healthcare concerns relative to urban populations. However, we don't consider the population density of a county. Different occupations more common in rural or urban areas may have a strong impact on mortality. Consider the lack of urban coal miners as an example. Ommissions like these create ommitted variable bias in our estimates of the effect of distance on mortality. In an extension, we would want to control for additional factors like these.

## Lack of within group variation in distance

Another concern is the lack of within group variation in hospital distance. While the Northern states have a lot of tracts with a large distance to the nearest hospital, the Southern states are relatively well covered by hospitals. Had we gone ahead with a regression showing within region effects of distance to the nearest hospital, the estimate for the Southern states would have been based soley on observations where people are relatively close whereas the Nothern states sample would have included many counties far from a hospital, biasing a comparison between the two. We may have wanted to select different groups of states such that each group had a similar range of distances to the nearest hospital.

## Other healthcare providers

hospitals are not the only providers in healthcare markets. Many of the counties we measure to be underserved by hospitals may have many small clinics or independent physician practices. Controlling for the prevelance of other healthcare providers would be critical in extending the analysis.

## Alternative choice for mortality not sensitive to distance

We measure mortality from Alzheimer's on the assumption that it would not be sensitive to distance so as to compare with a cause that is (heart failure). However, Alzheimer's could be correlated somewhat strongly with distance, and we could have made an alternative choice for a benchmark to compare with heart failure.

## Distance measure

we're measuring straight line distance in an Albers equal area projection of the states, but it would have been more accurate if we measured distance by road/travel distance instead. Ambulances do not travel as the crow flies.


