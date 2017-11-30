# Final_Project_DK_AG

# The Question

This project explores the impact of distance from a hospital on health outcomes and contrasts them against northern and southern states in the U.S. The northern states we used are Montana, North Dakota, and South Dakota. The southern states we used are Alabama, Louisiana, and Mississippi. Northern states are generally considered to have better health outcomes than southern states. Health outcomes are defined as deaths per 100,000 people. 

The initial hypothesis was that being farther from a hospital has a negative impact on health outcomes, and this negative impact would be larger in southern states than in northern states. By controlling for demographic variables such as age, race, income, sex, education, and unemployment rate, we hypothesized that distance is negatively correlated with health outcomes. Further, we expected distance to have a stronger negative correlation with health outcomes that are more sensitive to time/distance to hospitals, such as heart-related diseases, than health outcomes that are not sensitive to time/distance, such as Alzheimer's. However, after running our regressions, we do not find any significant correlations between health outcomes and distance. We offer several potential reasons in the later sections.

# The Data

To construct our analysis, we downloaded census tract centroid points for each state, coordinates for hospitals in our 6 states, demographic data, health outcomes, and a 2016 county crosswalk to merge our data. All of our data is from 2016.

Below is a list of the datasets we used.

* [2016 Census tract coordinates](https://www.census.gov/geo/maps-data/data/tiger-line.html)
* [Hospital coordinates](https://data.medicare.gov/Hospital-Compare/Hospital-General-Information/xubh-q36u)
* [2016 Demographic data (education, unemployment, median income)](https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t)
* [2016 Demographic data (sex, race, age)](https://wonder.cdc.gov/controller/datarequest/D76)
* [2016 Health outcomes](https://wonder.cdc.gov/controller/datarequest/D76)
* [2016 County crosswalk](http://www.nber.org/cbsa-msa-fips-ssa-county-crosswalk/2016/)

Our analysis occurs at the county level. The distance of a centroid to the closest hospital is averaged to the county level. All other data is collected at the county level. Education is expressed as percentage of total population within the specific education level. The unemployment rate is that for those over the age of 16. Sex and race are also expressed as percentages of total county population. We specified age to be percentage of total county population over the age of 65.

# The Investigation

All analysis code for this project is included in a single jupyter notebook.

[Health Outcomes and Distance](https://github.com/axgao1/Final_Project_DK_AG/blob/master/Final%20Project.ipynb)

The bulk of the analysis is performed in python, pandas and statsmodels.

We first obtained census tract centroid coordinates for each state using the downloaded shape files. We then converted the centroid coordinates and hospital coordinates to the same projection and found the minimum distance from each centroid to all hospitals, giving us the distance to the closest hospital from each centroid. 

Below is a map of our hospital locations limited to 1 northern state (Montana) and 1 southern state (Louisiana).

<img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/Hospital%20Locations%20with%20Distances.png?raw=true" width="500" height="500">

We then merged all of our demographic variables into one dataframe with health outcomes by county. Using this one combined dataframe, we were able to regress health outcomes on the demographics. 

Since we also decided to explore how distance impacts change with different causes of death, we created dataframes to only include heart-related diseases cause of death for each county and Alzheimer's-related cause of death for each county as contrasts against each other. Both dataframes are joined with all demographics and minimum distance data.

# Regression Analysis

## Baseline Model - All Cause Mortality Model

Our baseline model uses all causes of mortality (allcausemort) as the dependent variable. We regress log(allcausemort) on ged, highschool, somecollege, associates, bachelors, graduateplus, np.log(income), percentmale, nonhispanicwhite, percent65plus, unemployment, mindist, and state, as well as the minimum distance averaged at the county level. We hoped to demonstrate that being farther away from a hospital has a negative impact on overall health outcomes after having controlled for other variables that would also affect health outcomes.

## Heart-related Mortality Model

This model attempted to demonstrate how heart failure and heart-related causes of death are impacted by distance to nearest hospital. We regress log(heartmort) on ged, highschool, somecollege, associates, bachelors, graduateplus, np.log(income), percentmale, nonhispanicwhite, percent65plus, unemployment, mindist, and state, as well as the minimum distance averaged at the county level. We had hoped to demonstrate that being a kilometer away from a hospital has a larger negative impact on mortality after having controlled for other variables that would also affect health outcomes.

## Alzheimer's-related Mortality Model

This model attempted to demonstrate how Alzheimer's-related causes of death are impacted by distance to nearest hospital. We regress log(Alzheimers_mort) on ged, highschool, somecollege, associates, bachelors, graduateplus, np.log(income), percentmale, nonhispanicwhite, percent65plus, unemployment, mindist, and state, as well as the minimum distance averaged at the county level. We had hoped to demonstrate that being a kilometer away from a hospital has a smaller negative impact on mortality (in comparison to heart-related mortality) after having controlled for other variables that would also affect health outcomes.


Missing data - health outcomes


# The results

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

# Limitations of our Study and Possible Extensions

missing data on specific health outcomes (heart and alzheimers, in northern states especially)

hospital specializations - different capabilities, not all hospitals provide same services

other demographics that we dont control for - population density
  - people's occupation status (omitted variable bias) - concentrated in rural areas 
   (miner independently affects health outcomes but also determines how far you live from hospitals)

wider variations in distance to hospital in northern states (should have looked for states that have comparable distances to hospitals as northern states - not a lot of within group variation)

small clinics and independent practices (other health care providers) are not covered in our analysis (even if far from a hospital, if have clinic close by, it makes difference in health outcomes but we dont control for that)

maybe Alzheimer's death does depend on distance to hospital (if far from hospital, might not get medication as quickly)

we're measuring straight line distance in an Albers equal area projection of the states - but would have been more accurate if we measured distance by road/travel distance instead.
