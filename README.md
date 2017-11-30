# Final_Project_DK_AG

# The question
This project explores the impact of distance from a hospital on health outcomes and contrasts them against northern and southern states in the U.S. The northern states we used are Montana, North Dakota, and South Dakota. The southern states we used are Alabama, Louisiana, and Mississippi. Northern states are generally considered to have better health outcomes than southern states. Health outcomes are defined as deaths per 100,000 people. 

The initial hypothesis was that being farther from a hospital has a negative impact on health outcomes, and this negative impact would be larger in southern states than in northern states. By controlling for demographic variables such as age, race, income, sex, education, and unemployment rate, we hypothesized that distance is negatively correlated with health outcomes. Further, we expected distance to have a stronger negative correlation with health outcomes that are more sensitive to time/distance to hospitals, such as heart-related diseases, than health outcomes that are not sensitive to time/distance, such as Alzheimer's. However, after running our regressions, we do not find any significant correlations between health outcomes and distance. We offer several potential reasons in the later sections.

[Making a hyperlink](https://i.pinimg.com/736x/88/50/2b/88502b58b2ca3509be47473044fde8cc--wink-wink-adorable-animals.jpg)

images (go the image, right click to copy direct URL link you can resize using html options).

<img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/All%20Cause%20Data%20Availability%20Map.png?raw=true">

# The data

To construct our analysis, we downloaded census tract centroid points for each state, coordinates for hospitals in our 6 states, demographic data, health outcomes, and a 2016 county crosswalk to merge our data. 

Below is a list of the datasets we used.

[2016 Census tract coordinates] (https://www.census.gov/geo/maps-data/data/tiger-line.html)

education/unemployent/income 2016 - https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t
health outcomes/sex/race/age 2016 - https://wonder.cdc.gov/controller/datarequest/D76
2016 county crosswalk - http://www.nber.org/cbsa-msa-fips-ssa-county-crosswalk/2016/
census tract - https://www.census.gov/geo/maps-data/data/tiger-line.html (2016)
hospital address/coordinates - https://data.medicare.gov/Hospital-Compare/Hospital-General-Information/xubh-q36u

# The investigation
## 

Include link to jupyter notebook
Put in images/maps - save as pdf from notebook into directory and link to pdf in directory

We're regressing log(mortality) on various demographic variables as well as the mean distance from census tract centroids to the nearest hospital at the county level. We look at three measures of mortality:
All cause mortality
Mortality from heart failure and closely related heart conditions
Mortality from Alzheimers


We hoped to demonstrate that being farther from the hospital has a negative impact on health outcomes and that this impact is larger for heart-related deaths, particularly in the Southern states.

Use shape files to create tract dataframe - contains minimum distance column

Missing data - health outcomes


# The results

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
