# Final_Project_DK_AG
Final Project
# The question
We want to examine the difference in the impact of being far from a hospital on health outcomes in states where health outcomes are generally poor (LA, AL, MS) compared with states where they are generally good (MT, SD, ND).

We're doing a regression and including some other variables
# The data

put in links to websites
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
