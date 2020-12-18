Food, water and housing are fundamental requirements for daily living. Although housing is considered essential for well-being, little work has been done to assess the direct causal relationship between housing and housing improvement programs on health and welfare. In their study, Cattaneo et. al set out to study how floor quality impacts the health of young children and the mental health and happiness of their mothers. They assessed the causal relationship by utilizing a wide collection of data from the Piso Firme Project (PFP), a large-scale attempt from the Mexican government that aimed to replace dirt floors by cement floors.

PFP was first implemented locally in the state of Coahuila in 2000 and lasted until 2005. It was then gradually adopted in other states. The neighboring state, Durango, implemented the project only later and acted as a control region. To further enhance the comparability between treatment and control groups, the study was restricted to 3 municipalities located along the border: Torreón in the state of Coahuila (treatment) and Gómez Palacio and Lerdo in the state of Durango (control). All regions had similar socio-economical characteristics.

{% include pre_treatment_proportions.html %}

Actually, there is one small difference above between the proportions observed in each census in both states! Can you spot it? (*Hint: try to zoom in on the second comparison*)

{% include pre_treatment_average.html %}

Before assessing the causal relationship between flooring quality and health and welfare, we have to make sure that PFP succeeded in providing adequate cement flooring. Prior to the beginning of the project in Coahuila, all three municipalities of interest had the same percentage share of rooms with cement floors in households. After the project, although all three regions of interest experienced an increase in cement flooring, Torreón showed a significantly increased percentage share of rooms with cement floors. 

{% include map_with_button.html %}



The effect of the housing improvement program on maternal happiness was then assessed through linear regressions of main variables of maternal happiness on a program dummy. All coefficients were statistically significant and the following results were observed:

**Dependent variable** | **Control group** | **Model 1** | **Model 2** | **Model 3**
--- | --- | --- | --- | --- |
*Satisfcation with floor quality* | 0.511 (0.5) | 0.219 [0.023]*** | 0.223 [0.024]*** | 0.222 [0.026]***
*Satisfcation with house quality* | 0.605 (0.489) | 0.092 [0.021]*** | 0.087 [0.021]*** | 0.084 [0.022]*** 
*Satisfcation with life quality* | 0.601 (0.490) | 0.112 [0.022]*** | 0.112 [0.021]*** | 0.112 [0.022]*** 
*Depression scale* | 18.532 (9.402) | -2.315 [0.616]*** | -.,417 [0.570]*** | -2.372 [0.562]***
*Perceived stress scale* | 16.514 (6.914) | -1.751 [0.428]*** | -1,769 [0.396]*** | -1.742 [0.396]***


Overall, the following observations were made:

*	**Satisfaction** with *floor quality*, *house quality* and *life quality* were significantly **increased** for mothers in households which received PFP

*	**Depression** and **stress levels** significantly **decreased** for mothers in household which received PFP

While these effects demonstrated the benefits of the housing improvement program in all household that received it, they failed to account for differences in initial levels of cement flooring between household prior to the beginning of the project. Yes, the project was beneficial but… 

...**are there differences in terms of how beneficial the project was for maternal happiness based on the initial levels of cement in the household?** 

This is the question we seek to answer, and this is where our story begins…. Bear with us as we take you through our journey!


## Feature engineering a Happy variable

The initial analysis in “Housing, Health and Happiness” analyzed the effect of PFP by regressing several maternal mental health measures on the program dummy. In order to gain deeper insight into all variables at once, the first thing we did was feature engineer one variable that would encompass all measures of maternal mental health and tell us if the mother in the household was happy or not: the “Happy” variable. Since there was no actual label indicating whether a household mother was happy or not, we used several unsupervised machine learning approaches. The one that gave the optimal results was K-Means with 2 clusters. 

The K-Means clustering algorithm was run using the following satisfaction and maternal mental health measures:

* **Satisfaction with floor quality** (dummy variable)

* **Satisfaction with house quality** (dummy variable)

* **Satisfaction with life quality** (dummy variable)

* **Perceived stress scale** (numerical variable)

* **Depression scale** (numerical variable)

The K-Means algorithm identified two clusters and based on the classification, the *Happy* variable was created to be equal to **1** if the household mother was happy and **0** otherwise. The clusters and their labels can be visualised on the perceived stress vs the depression scale scatterplot below:

{% include scatter_agglomerative.html %}

Although a distinct separation does not appear, the clusters are intuitively correct. As perceived stress and depression increase, the chances of the mother being unhappy increase. The two clusters had significanlty differents distributions of perceived stress scales and depression scales, which further reinforced the idea that K-Means succeded in differentiating between happy and unhappy mothers:

**Boxplot KMeans PSS et CESDS**

The K-Means algorithm did not rely only on the numerical variables to identify happy and unhappy mothers. Mothers in both households had significantly different mean levels of satisfaction regarding floor quality, house quality and life quality:

**Scatter plot mean levels of satisfaction**

## Propensity Score Matching of Households that received PFP

The befenits for the households that received PFP compared to those that did not are already established. Our focus now is on differences between households that received PFP, but had different initial level of cement coverage. To ensure that we analyse the true treatment effect, we have to compare similar households. Because there was no measure of pre-project percent share of cemented floors for individual households, we created a dummy variable that was equal to **1** if the share of cement floors was **above the median** observed among treated housholds prior to the project and **0** if it was below the median.

Once the **dummy_cement** variable was computed, we calculated the propensity scores of having a percent share of cement floors above the median for each treated household. We used 3 different approaches to compute the propensity score:

1. **Random Forest Classifier**
2. **XGBoosting Classifier**
3. **Decision Trees**

**FIGURE PROPENSITY SCORES DES 3 CLASSIFIERS**

Once the propensity scores were computed, we matched treated household with similar scores (**epsilon < 0.05**). The different methods gave different matching couples.

**FIGURE MATCHING 3D**


