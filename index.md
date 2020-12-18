Food, water and housing are fundamental requirements for daily living. Sadly, it has been estimated that around 600 million people globally are exposed to serious health risk due to poor housing, with children being particularly vulnerable. Adequate housing with access to water supply, sanitation and ventilation has long been considered essential for a person's well-being and most countries in the world have devoted substantial resources to improve these forsaken urban-areas. Nonetheless, little work has been done to assess the causal relationship between housing, and housing improvement programs, and health and welfare. In 2000, the Mexican state of Coahuila, launched a program to replace dirt floors with cement ones. The program was called Piso Firme (PFP), and it provided more than 34,000 homes with cement flooring by 2005. The success in Coahuila, eventually led to other states adopting the program as well.<br/>

In their [study](https://www.aeaweb.org/articles?id=10.1257/pol.1.1.75), Cattaneo et. al set out to assess how the Mexican program to replace dirt floors with cement floors impacted **children health and theirs mothers' mental health and happiness**. To that end, the team used the results from Coahuila and its neighboring state, Durango, which implemented the project only later and acted as a control region. To further enhance the comparability between treatment and control groups, the study was restricted to 3 municipalities located along the states border: Torreón in the state of Coahuila (treatment) and Gómez Palacio and Lerdo in the state of Durango (control). All regions had similar socio-economical characteristics, as shown on the figures below.<br/>

{% include pre_treatment_proportions.html %}

Actually, there is one small difference above between the proportions observed in each census in both states ! Can you spot it? (*Hint: try to zoom in on the second comparison*)

{% include pre_treatment_average.html %}

Before assessing the causal relationship between flooring quality and health and welfare, we have to make sure that PFP succeeded in providing adequate cement flooring. Prior to the beginning of the project in Coahuila, all three municipalities of interest had the same percentage share of rooms with cement floors in households. After the project, although all three regions of interest experienced an increase in cement flooring, Torreón showed a significantly higher increase in the share of rooms with cement floors per households. <br/>

{% include map_with_button.html %}



The effect of the housing improvement program on maternal happiness was then assessed through linear regressions of main factors of maternal happiness (*Satisfaction with floor quality, Perceived stress, etc*), on a program dummy (Treated or Control). All coefficients were statistically significant and the following results were observed:

**Dependent variable** | **Control group** | **Model 1** | **Model 2** | **Model 3**
--- | --- | --- | --- | --- |
*Satisfcation with floor quality* | 0.511 (0.5) | 0.219 [0.023]*** | 0.223 [0.024]*** | 0.222 [0.026]***
*Satisfcation with house quality* | 0.605 (0.489) | 0.092 [0.021]*** | 0.087 [0.021]*** | 0.084 [0.022]*** 
*Satisfcation with life quality* | 0.601 (0.490) | 0.112 [0.022]*** | 0.112 [0.021]*** | 0.112 [0.022]*** 
*Depression scale* | 18.532 (9.402) | -2.315 [0.616]*** | -.,417 [0.570]*** | -2.372 [0.562]***
*Perceived stress scale* | 16.514 (6.914) | -1.751 [0.428]*** | -1,769 [0.396]*** | -1.742 [0.396]***


Overall, the following observations were made:

*	**Satisfaction** with *floor quality*, *house quality* and *life quality* were significantly **increased** for mothers in households which received PFP. <br/>
*	**Depression** and **stress levels** significantly **decreased** for mothers in household which received PFP. <br/>

While these effects demonstrated the benefits of the housing improvement program in all households that received it, they failed to account for differences in initial levels of cement flooring among households prior to the beginning of the project. Yes, the project was beneficial but… 

...**are there differences in terms of how beneficial the project was for maternal happiness based on the initial levels of cement in the household?** 

This is the question we seek to answer, and this is where our story begins…. Bear with us as we take you through our journey!


## Feature engineering: a Happy variable

The initial analysis in *Housing, Health and Happiness* analyzed the effect of PFP by regressing several maternal mental health measures on the program dummy. In order to gain deeper insight into all variables at once, we engineered a variable that would encompass all measures of maternal mental health and determine if the mother in the household was happy or not: the **Happy** variable. Since there was no actual label indicating how a mother was actually feeling, we used several unsupervised machine learning approaches. The most optimal output was obtained with K-means resulting in two clusters: Happy and Unhappy. <br/>

The K-Means clustering algorithm was run using the following satisfaction and maternal mental health measures:

- **Satisfaction with floor quality** (dummy variable)

- **Satisfaction with house quality** (dummy variable)

- **Satisfaction with life quality** (dummy variable)

- **Perceived stress scale** (numerical variable)

- **Depression scale** (numerical variable)

The K-Means algorithm identified two clusters and based on the classification, the *Happy* variable was created to be equal to **1** if the household mother was happy and **0** otherwise. The clusters and their labels can be visualised on the *Perceived stress* vs. *Depression levels* scatterplot below:

{% include scatter_KM.html %}

Although a distinct separation does not appear, the clusters are intuitively correct. As perceived stress and depression increase, the chances of the mother being unhappy increase. The two clusters had significanlty different distributions of perceived stress scales and depression scales, which further suggested that K-Means succeded in differentiating between happy and unhappy mothers:

{% include violin_KM.html %}

The K-Means algorithm did not rely only on the numerical variables to identify happy and unhappy mothers. Mothers in both households had significantly different mean levels of satisfaction regarding floor, house and life quality:

{% include mean_satisfaction_agglomerative.html %}

We can see that the clustering into the happy and the unhappy groups is also efficient regarding the numerical variables. The differences of means  between each of these 3 variables is sufficient to reinforce the confidence we have in these clusters. 

## Propensity Score Matching of Households that received PFP

It has been established that households which received PFP had improved children health and mothers happiness. However, among said households, the initial share of cements in the house varied greatly. To ensure that we analyse the true treatment effect, we have to compare similar households in terms of initial cement coverage. Unfortunately, there was no measure of pre-project share of cemented floors for individual households. As such, we created the **Cement** variable which was equal to **1** if the share of cement floors was **above the median** observed among treated housholds prior to the project and **0** if it was below the median.

Once the Cement variable was computed, we calculated the propensity scores of having a share of cement floors above the median for each treated household. We used 3 different approaches to compute the propensity score:

1. **Random Forest Classifier**
2. **XGBoosting Classifier**
3. **Decision Trees**

{% include cement_classifier_all.html %}

Once the propensity scores were computed, we matched treated household with similar scores (**epsilon < 0.05**). The different methods gave different matching couples.

{% include sample_matching.html %}

In this 3D illustration, we randomly chose 4 households and we represented them in the space defined by 3 of the 4 variables used to compute the propensity score. The symbols represent the different classifiers used for the different matchings, and the colors characterize the 4 sampled households. We see that for all classifers, the matches are close to each other on the three dimensions shown here. This suggests that the matching was well performed. For instance, the green (and red) household was matched to the same untreated household by two classifiers. <br/>

Finally, we can study the **average treatment effect** obtained by each classifier, for each clustering strategy (K-Means and Agglomerative), to study whether or not the causal relationship determined by Cattaneo et. al is co-founded by the initial share of cement in the household. <br/>

{% include ATE_final.html %}


As shown above, the scores obtained are all negative which suggests that households with the lowest share of cement coverage prior to PFP demonstrate a higher increase in happiness following the project. However, the lowest score being -0.048, the effect is probably not very significant. 
