# The Impact of Distance to a Hospital on Regional Health Outcomes in the US

# The Question

This project explores the impact of distance from a hospital on health outcomes and contrasts the impact in northern states against that of southern states in the U.S. The northern states we used are Montana, North Dakota, and South Dakota. The southern states we used are Alabama, Louisiana, and Mississippi. Northern states are generally considered to have better health outcomes than southern states. Health outcomes are defined as deaths per 100,000 people. 

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


Below is a map indicating the availability of our data for all counties' all causes of death.

<img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/All%20Cause%20Data%20Availability%20Map.png?raw=true" width="500" height="500">



# Regression Analysis

## Baseline Model - All Cause Mortality Model

Our baseline model uses all causes of mortality (allcausemort) as the dependent variable. We regress log(all cause mortality) on percent of the population with a ged, percent of the population with a high school diploma, percent of the population with some college, percent of the population with an associates degree, percent of the population with a bachelors, percent of the population with a graduate degree or higher, log(median income), percent of the population that is male, percent of the population that is non-hispanic white, percent of the population that is 65+, U3 unemployment, the average minimum distance to a hospital in the county, and state. We hoped to demonstrate that being farther away (a kilometer further) from a hospital increases mortality after having controlled for other variables that would also affect health outcomes.

Our other two regressions had the same independent variables and function form, but used log(heart condition mortality) and log(Alzheimer's mortality) as their dependent variables. We hypothesized finding that mortality from heart conditions would be more sensative to hospital distance than either all cause mortality or mortality from Alzheimer's. 

## Heart-related Mortality Model

This model attempted to demonstrate how heart failure and heart-related causes of death are impacted by distance to nearest hospital. We regress log(heartmort) on ged, highschool, somecollege, associates, bachelors, graduateplus, np.log(income), percentmale, nonhispanicwhite, percent65plus, unemployment, mindist, and state, as well as the minimum distance averaged at the county level. We had hoped to demonstrate that being farther away (a kilometer further) from a hospital has a larger negative impact on mortality after having controlled for other variables that would also affect health outcomes.

## Alzheimer's-related Mortality Model

This model attempted to demonstrate how Alzheimer's-related causes of death are impacted by distance to nearest hospital. We regress log(Alzheimers_mort) on ged, highschool, somecollege, associates, bachelors, graduateplus, np.log(income), percentmale, nonhispanicwhite, percent65plus, unemployment, mindist, and state, as well as the minimum distance averaged at the county level. We had hoped to demonstrate that being a kilometer away from a hospital has a smaller negative impact on mortality (in comparison to heart-related mortality) after having controlled for other variables that would also affect health outcomes.

Given that 

# The Results

Overall, we hoped to demonstrate that being farther from the hospital has a negative impact on health outcomes and that this impact is larger for heart-related deaths, particularly in the Southern states.

Regressions and what they show
Overall death
Heart Death
Alzehmiers death

nothing signficant

all cause mortality - education was significant (except for associates degree) makes you die less
being old makes you die more (obviously) - age

heart related death is closer to being significant 
alzheimers related death is closer to beign significant (which we expect to not be sensitive to distance)
this means we want to focus on specific health outcomes/reasons for death instead of all cause mortality

We did not test for differences between min distance in northern and southern states because min distance has such small explanatory power - would not have been useful because we have such insignificant results for min distance

# Limitations of our Study and Possible Extensions [WILL ADD BOTH MISSING DATA MAPS]

missing data on specific health outcomes (heart and alzheimers, in northern states especially)

hospital specializations - different capabilities, not all hospitals provide same services

We also did not control for differences between hospitals and treated hospitals as homogeneous entities. In reality, different hospitals provide vastly different quality of service and many lack particular capabilities all together. An extension of this study would attempt to more richly model what hospitals can and cannot do as well as the quality of service they provide. 

other demographics that we dont control for - population density
  - people's occupation status (omitted variable bias) - concentrated in rural areas 
   (miner independently affects health outcomes but also determines how far you live from hospitals)

Additionally, we should have controlled for other demographic variables that may be correlated both with distance from a hospital and health outcomes. For example, rural populations likely face different healthcare concerns relative to urban populations. However, we don't consider the population density of a county. Different occupations more common in rural or urban areas disntant from hospitals may have a strong impacts on mortality. Consider the lack of urban coal miners as an example.

wider variations in distance to hospital in northern states (should have looked for states that have comparable distances to hospitals as northern states - not a lot of within group variation)

Another concern is the lack of within group variation in hospital distance. While the Northern states have a lot of tracts with a large distance to the nearest hospital, the Southern states are relatively well covered by hospitals. Had we gone ahead with a regre

small clinics and independent practices (other health care providers) are not covered in our analysis (even if far from a hospital, if have clinic close by, it makes difference in health outcomes but we dont control for that)

maybe Alzheimer's death does depend on distance to hospital (if far from hospital, might not get medication as quickly)

we're measuring straight line distance in an Albers equal area projection of the states - but would have been more accurate if we measured distance by road/travel distance instead.
