# The Impact of Distance to a Hospital on Regional Health Outcomes in the US

# The Question

This project explores the impact of distance from a hospital on health outcomes and contrasts the impact in Northern states against that of southern states in the U.S. The northern states we used are Montana, North Dakota, and South Dakota. The southern states we used are Alabama, Louisiana, and Mississippi. Northern states are generally considered to have better health outcomes than southern states. Health outcomes are defined as deaths per 100,000 people. 

The initial hypothesis was that being farther from a hospital has a negative impact on health outcomes, and this negative impact would be larger in southern states than in northern states. After controlling for demographic variables such as age, race, income, sex, education, and unemployment rate, we hypothesized that distance is negatively correlated with health outcomes. Further, we expected distance to have a stronger negative correlation with particular health outcomes that are more sensitive to time/distance to hospitals, such as heart-related diseases relative to health outcomes that are not as sensitive to time/distance, such as Alzheimer's. However, after running our regressions, we do not find any significant correlations between health outcomes and distance. We offer several potential reasons in the later sections.

# The Data

To construct our analysis, we downloaded census tract shape files for each state from the Census Bureau, coordinates for hospitals in our 6 states from Medicare's Compare website, health outcomes from CDC Wonder, demographic data from the ACS and CDC Wonder, and a 2016 county FIPS to county name crosswalk from the NBER for the purpose of merging our data. All of our data is from 2016.

Below is a list of the datasets we used.

* [2016 Census tract coordinates](https://www.census.gov/geo/maps-data/data/tiger-line.html)
* [Hospital coordinates](https://data.medicare.gov/Hospital-Compare/Hospital-General-Information/xubh-q36u)
* [2016 Demographic data (education, unemployment, median income) from ACS](https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t)
* [2016 Demographic data (sex, race, age) from CDC](https://wonder.cdc.gov/controller/datarequest/D76)
* [2016 Health outcomes](https://wonder.cdc.gov/controller/datarequest/D76)
* [2016 County crosswalk](http://www.nber.org/cbsa-msa-fips-ssa-county-crosswalk/2016/)

Our analysis occurs at the county level. The distance of a census tract centroid to the closest hospital is averaged across all tracts in a county to approximate a county level distance to hospital metric. All other data is collected at the county level. Education is expressed as percentage of total population within the specific education level. The unemployment rate is that for those over the age of 16. Sex is also expressed as percentages of total county population. Race is presented as percentage of the population that is non-hispanic white. We specified age to be percentage of total county population over the age of 65.

# The Investigation

All analysis code for this project is included in a single jupyter notebook.

[Health Outcomes and Distance](https://github.com/axgao1/Final_Project_DK_AG/blob/master/Final%20Project.ipynb)

The bulk of the analysis is performed in python, pandas and statsmodels.

We first obtained census tract centroid coordinates for each state using the downloaded shape files. We then converted the centroid coordinates and hospital coordinates to an Alber's Equal Area projection and found the minimum distance from each centroid to a hospital. 

Below is a map of hospital locations with tracts colored by the distance of their centroids to the nearest hospital for the northern states, Montana, North Dakota, South Dakota, and the southern states, Louisiana, Alabama, and Mississippi.

<p align="center">
  <img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/Hospital%20Locations%20with%20Distances.png?raw=true"     width="600" height="600">
</p>

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

## All Cause Mortality Regression

| Dep. Variable: | np.log(allcausemort) | R-squared:	      | 0.610 |
---------------- | -------------------- | ----------------- | ----- |
Model     | OLS      |Adj. R-squared:  | 0.592
Method   | Least Squares  | F-statistic: | 33.63
Date: |	Tue, 28 Nov 2017 |	Prob (F-statistic): |	3.35e-64
Time:	| 15:43:28 |	Log-Likelihood:	| 218.27
No. Observations:	| 384	| AIC:	| -400.5
Df Residuals:	| 366	| BIC:	| -329.4
Df Model:|	17		
Covariance Type:|	nonrobust	

|  |	coef | std err |	t	| P>abs(t)	| [0.025 |	0.975] |
--------| ---- | ------- | -- | --------- | ------ | ------- |
Intercept	| 10.0629 |	0.616 |	16.340 |	0.000 |	8.852|	11.274
state[T. Louisiana]|	-0.0169|	0.029|	-0.581|	0.562|	-0.074|	0.040
state[T. Mississippi]|	-0.0409|	0.025|	-1.645|	0.101|	-0.090|	0.008
state[T. Montana]|	-0.0267|	0.039| -0.679| 0.498| -0.104| 0.051
state[T. North Dakota]|	0.0519|	0.044|	1.187|	0.236|	-0.034|	0.138
state[T. South Dakota]|	0.0129|	0.035|	0.363|	0.716|	-0.057|	0.083
ged|	-0.2974|	0.590|	-0.504|	0.614|	-1.457|	0.862
highschool|	-0.6531|	0.282|	-2.318|	0.021|	-1.207|	-0.099
somecollege|	-0.1294|	0.319|	-0.406|	0.685|	-0.757|	0.498
associates|	0.2172|	0.441|	0.493|	0.622|	-0.650|	1.084
bachelors|	-1.4434|	0.364|	-3.970|	0.000|	-2.158|	-0.728
graduateplus|	-1.4080|	0.424|	-3.319|	0.001|	-2.242|	-0.574
np.log(income)|	-0.2043|	0.059|	-3.464|	0.001|	-0.320|	-0.088
percentmale|	-1.5309|	0.428|	-3.574|	0.000|	-2.373|	-0.689
nonhispanicwhite|	-0.1203|	0.066|	-1.820|	0.070|	-0.250|	0.010
percent65plus|	2.8215|	0.266|	10.621|	0.000|	2.299|	3.344
unemployment|	-0.0098|	0.003|	-3.662|	0.000|	-0.015|	-0.005
mindist|	-2.456e-05|	0.000|	-0.117|	0.907|	-0.000|	0.000

This counter intuitive result is also borne out in the heart-related mortality regression. The coefficient on mindist in this regression is -0.0012, although this is still insigificant. This is especially surprising because we expected heart-related mortality to be extremely sensitive to distance from a hospital. When a person is having a heart attack, we would expect distance and time to a hospital to be critical for survival rate. In our results, we only explain 36.2% of the variation in heart-related mortality, so our model is missing key factors.

## Heart-Related Mortality Regression

| Dep. Variable: | np.log(heartmort) | R-squared:	      | 0.362 |
---------------- | -------------------- | ----------------- | ----- |
Model:|	OLS	|Adj. R-squared:|	0.301
Method:|	Least Squares|	F-statistic:|	5.906
Date:|	Tue, 28 Nov 2017|	Prob (F-statistic):|	1.24e-10
Time:|	15:43:28|	Log-Likelihood:|	-69.831
No. Observations:|	195|	AIC:|	175.7
Df Residuals:|	177|	BIC:|	234.6
Df Model:|	17		
Covariance Type:|	nonrobust		

| | coef|	std err|	t	P>abs(t)|	[0.025|	0.975]|
--| ----| ------ | ---------- | ----- | ------|
Intercept|	2.7361|	2.773|	0.987|	0.325|	-2.736|	8.208
state[T. Louisiana]|	0.1113|	0.103|	1.079|	0.282|	-0.092|	0.315
state[T. Mississippi]|	0.0899|	0.082|	1.096|	0.275|	-0.072|	0.252
state[T. Montana]|	-0.2674|	0.192|	-1.395|	0.165|	-0.646|	0.111
state[T. North Dakota]|	0.0652|	0.212|	0.308|	0.759|	-0.353|	0.483
state[T. South Dakota]|	-0.0829|	0.181|	-0.457|	0.648|	-0.441|	0.275
ged|	1.7312|	2.654|	0.652|	0.515|	-3.506|	6.969
highschool|	0.3882|	1.251|	0.310|	0.757|	-2.081|	2.858
somecollege|	0.6860|	1.371|	0.501|	0.617|	-2.019|	3.391
associates|	-1.9570|	2.218|	-0.882|	0.379|	-6.335|	2.421
bachelors|	-2.9207|	1.710|	-1.708|	0.089|	-6.296|	0.454
graduateplus|	3.8749|	2.216|	1.749|	0.082|	-0.498|	8.247
np.log(income)|	0.2261|	0.251|	0.901|	0.369|	-0.269|	0.721
percentmale|	-4.2652|	2.334|	-1.828|	0.069|	-8.870|	0.340
nonhispanicwhite|	0.3600|	0.255|	1.410|	0.160|	-0.144|	0.864
percent65plus|	7.2137|	1.411|	5.113|	0.000|	4.430|	9.998
unemployment|	-0.0001|	0.012|	-0.010|	0.992|	-0.025|	0.025
mindist|	-0.0012|	0.001|	-1.225|	0.222|	-0.003|	0.001

In examining the Alzheimer's-related mortality regression, the coefficient on mindist (0.0016) is positive as expected, although this is still insignificant. We do expect that being a kilometer further from the nearest hospital is correlated with higher mortality due to Alzheimer's. Ironically, the measure we expect to be the least sensitive to distance from hospital seems to be the most sensitive with a correct correlation direction and the highest t-statistic of the three coefficients.

## Alzheimer's-related Mortality Regression

| Dep. Variable: | np.log(Alzheimers_mort) | R-squared:	      | 0.501 |
---------------- | -------------------- | ----------------- | ----- |
Model:|	OLS|	Adj. R-squared:|	0.446
Method:|	Least Squares|	F-statistic:|	9.143
Date:|	Tue, 28 Nov 2017|	Prob (F-statistic):|	4.05e-16
Time:|	15:43:28|	Log-Likelihood:|	-69.803
No. Observations:| 173| AIC:|	175.6
Df Residuals:|	155|	BIC:|	232.4
Df Model:|	17		
Covariance Type:|	nonrobust		

| | coef|	std err|	t	P>abs(t)|	[0.025|	0.975]|
--| ----| ------ | ---------- | ----- | ------|
Intercept|	2.3775|	3.166|	0.751|	0.454|	-3.877|	8.632
state[T. Louisiana]|	0.1811|	0.111|	1.637|	0.104|	-0.037|	0.400
state[T. Mississippi]|	0.1476|	0.092|	1.610|	0.109|	-0.034|	0.329
state[T. Montana]|	-0.6499|	0.221|	-2.936|	0.004|	-1.087|	-0.213
state[T. North Dakota]|	0.3551|	0.243|	1.461|	0.146|	-0.125|	0.835
state[T. South Dakota]|	0.3084|	0.212|	1.458|	0.147|	-0.110|	0.726
ged|	-0.4713|	3.154|	-0.149|	0.881|	-6.702|	5.760
highschool|	0.3245|	1.594|	0.204|	0.839|	-2.824|	3.473
somecollege|	-0.3328|	1.438|	-0.232|	0.817|	-3.173|	2.507
associates|	4.3131|	2.469|	1.747|	0.083|	-0.564|	9.190
bachelors|	-1.5957|	2.003|	-0.797|	0.427|	-5.552|	2.360
graduateplus|	1.6575|	2.364|	0.701|	0.484|	-3.013|	6.327
np.log(income)|	-0.0817|	0.296|	-0.276|	0.783|	-0.666|	0.503
percentmale|	2.1038|	2.549|	0.826|	0.410|	-2.930|	7.138
nonhispanicwhite|	-0.7672|	0.285|	-2.692|	0.008|	-1.330|	-0.204
percent65plus|	10.1084|	1.422|	7.110|	0.000|	7.300|	12.917
unemployment|	-0.0114|	0.016|	-0.727|	0.468|	-0.042|	0.020
mindist|	0.0016|	0.001|	1.350|	0.179|	-0.001|	0.004

## Analysis of covariates
In examining the state fixed effects, it appears that the differences between states are not as large as we expected. No state's mortality outcomes are significantly different from Alabama, except for Montana in the Alzheimer's-related mortality regression, where Montana has significantly lower Alzheimer's-related mortality than Alabama. Though insignificant, North Dakota and South Dakota generally tend to have higher mortality than Alabama. All of this contradicts our initial assumption that Northern states have better health outcomes than Southern states. However, we did not rigorously test the difference in health outcomes between Northern and Southern states, a potential limitation to our study commented on below.

Reviewing the rest of our control variables, we see that age is the biggest predictor in causes of mortality, especially since we limited it to population above 65. Having more old people leads to more deaths. 

# Limitations of Our Study and Possible Extensions 

## Missing data

The first limitation to our study is the fact that, though we had complete data for all cause mortality, our data by cause of mortality is highly fragmentary. This can be easily seen in our maps below showing data availability. There is a lot of missing data in the Northern states especially. Without this data, our effective number of observations is extremely limited as counties with missing mortality rates are excluded from our regression results, making our results inaccurate. We tend to be missing more data in rural areas far from hospitals, which compounds the problem. An extension of this study would need to use more complete data for mortality by specific causes.

<p align="center">
  <img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/All%20Cause%20Data%20Availability%20Map.png?raw=true" width="600" height="600" align="middle">
</p>

<p align="center">
  <img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/Heart%20Data%20Availability%20Map.png?raw=true" width="600" height="600" align="middle">
</p>

<p align="center">
  <img src="https://github.com/axgao1/Final_Project_DK_AG/blob/master/Alzheimer's%20Data%20Availability%20Map.png?raw=true" width="600" height="600" align="middle">
</p>

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
