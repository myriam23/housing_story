<!---
**With more than 14'000 restaurants, food is a serious business in Chicago. But choosing where to eat is not an easy task, especially when you want to avoid risky places. Our choices are usually affected by the reviews of other visitors and the location of the restaurant. But do we ever wonder if a restaurant fulfils all sanitary standards?**

Using [Chicago Food Inspection data](https://www.kaggle.com/chicago/chicago-food-inspections), our goal is to provide insights into food quality in the Windy City. We want to explore what are the violations that restaurants make most often, how they change over time and are they connected to the area where restaurant is located.
--->
Imagine that you are traveling to Chicago next week. Should it be for business or leisure, you'll be staying in Chicago for four days and you would like to discover the local gastronomy. A morning coffee with a lake view, then lunch with an old friend in a local hidden gem place and, at the end of the day a nice and romantic dinner. 

But imagination is not enough to make such a perfect day happen! If you want perfect meals all day long, you have to spend some time figuring out which places you want to visit. This can often be a challenging and frustrating task, especially in an unknown city. 

**So, what would be your approach to find a good restaurant?** For sure, Google will be your friend in this exploration. Nowadays, few people visit places without checking user reviews on TripAdvisor and Facebook, and the magnificent pictures of food that restaurants publish on their Instagram profiles are often used as selection criteria. And let's not forget friends of friends of friends who have recently been in Chicago, who can often offer useful recommendations.

Okay, those are pretty much standard approaches. Why not try something different? 

What if we tell you that we looked at **restaurant's inspection results** to figure out where to eat in Chicago? That's right, thousands of inspection reports with notes of poor inspectors relating their encounters with cockroaches in pantries, or that they closed restaurants because they lacked defined vomiting procedures. When going to a restaurant, you rarely wonder if it fullfils all standards issued by the authorities. However, inspection results can be used as a non-standard approach to the "where-to-eat-tonight" problem, and are very useful to assess which restaurants in the Windy City are worth visiting. 

The dataset we used contains information from inspections of restaurants and other food establishments in Chicago in the past ten years. Every inspection comes with basic information about the restaurant such as name, licence number, location, zip code, then the type of the inspection that was performed and, of course, the result of the inspection followed by a description of the violations discovered.

Let's start with a quick look on the inspection results of the past ten years.

{% include results_of_inspections_per_year.html %}

It seems that majority of the restaurants managed to pass their inspections before 2018 but afterwards we can see increase in inspections that passed with conditions. What may have caused that? It is clearly not enough to look just at pass and fail we would like to discover what are the reasons behind inspection failure.

<h2>Violations</h2>

So, if we really want to figure out how good a restaurant is, based on the inspection results, common sense tells us that if a restaurant commits many violations, we might reconsider our decision to visit it. That is why we wanted to check how many violations restaurants make on average per inspection. But looking at several thousand of restaurants at a time isn't really possible, so we first group all of the restaurants based on their location. The city of Chicago is officially divided into 77 community areas, which are grouped into 9 districts. We used this geographical and administrative partition of the city as a starting point and checked how many violations per inspection restaurants make on average in each community area. 

<div class="map">
{% include nb_violations_area.html %}
</div>

The community areas with the highest number of violations are West Ridge, South Lawndale, Washington Park and Chatham. We can see an area in the south of Chicago with noticeably high numbers of violations, made of Chatham, Roseland, West Pullman, Wasington Heights, and Morgan Park. The lakeside community areas, on the other hand, contain the safest restaurants: Loop, Near South side and Douglas.
Are areas likely to change their number of violations over time? Let's take a look at how the number of violations evolves over time, for each community area.

<div class="map">
{% include nb_violations_evolution.html %}
</div>

In most areas, the number of violations observed has decreased. Surprisingly, the few areas who have seen their number increase are not the ones with the highest violation numbers: Near West Side and Logan Square, though definitely not the areas with least violations, were not the worst either as we've seen previously.

Now we know the numbers, but we still don't know what hides behind them. Observed violations are very diverse: they can represent issues with the food, with the facilities or the training of the personnel. Knowing which violations are the most prevalent and where they are mainly observed could help us pick the best areas. 
Contrary to what one might think, the number of violations is not always a good indicator of the inspection result. Indeed, some passing restaurants have comitted up to 13 violations, while some failing restaurants have less than 4 violations.

**One note before we dive into the analysis:** Chicago changed their violation definitions in the middle of 2018. Before the change an establishment could receive one or more of 45 distinct violations. After the change, the violations were redefined and their number increased to 59. The inspections also became much stricter and inspectors are now more likely to deliver just "Pass with conditions" results rather than a clear "Pass" if some criteria are violated. That is why in the analysis we will usually look at those two periods separately.
 
Although every violation describes some specific shortcoming of a restaurant, all of them, both before and after the change, can be grouped into 5 general categories:

1. **Food violations:** violations that are related to the food and ingredients: the way they are obtained, stored, transported, prepared, labeled etc.
2. **Facility conditions violations:** presence of appropriatelly set up equipment and utilities (freezer, ovens, work surface etc.), as well as requirements for the building and infrastructure (lighting, ventilation, temperature, pipes, walls etc.).
3. **Sanitary related violations:** violations that are related to keeping all the equipment, rooms and surfaces clean .
4. **Staff related violations:** violations related to the employees (necesarry trainings, the way they work with food, manager's work etc.).
5. **Other:** the violations that did not match the previous groups (eg. summary report of the inspection visible to the public, no smoking regulations etc.).

Now, being familiar with that, let's dive into the analysis!

First let's see if there are any patterns of the most common violations noted in Chicago. As mentioned, we can look at Chicago before the change of violations and after it.

<div class="map">
{% include most_common_violations.html %}
</div>

Before the July of 2018 the predominant violation in most Chicago communities is *Proper floors maintenance*. Between Cicero and Oak Loan in the south-west side of Chicago you can also find a region of 7 communities, for whose restaurants the *Clean equipment* criterion was violated the most.

After the change of violations restaurants in the huge part of the middle and southern bay area as well as the region between the New City and Greater Grand Crossing most oftenly violated *Adequate management* criterion - this concerns regulations about personnel hierarchy and their reporting procedures. There are also regions in the north, south and west parts of Chicago where *Procedures for reacting to customer sickness* was the most common issue.

<!-- Number of inspections tells us how many different fouls restaurant made, but it doesn't give any information about the core of the violation. Was it some life-threathening violation, or hazardous one? -->

Now, let's see what are the most frequent violations. Since our goal is to filter bad restaurants, we will focus only on the violations which were noticed in inspections that ended with failure. Below are top 25 most common violations in failed inspections before 1.7.2018.

{% include violations_failed_before.html %}

We have a winner! As it was also seen on community maps of all violations, also among the failed inspections only, dirty floor was mentioned the most as one of the violations. Two out of top three violations are related to cleaningness in the restaurant. Interestingly, many restaurants have problems with walls and ceiling constructions. Of course, there could be that some part of ceiling and wall problems is also due to dirtiness apart from construction.

It seems that "most popular violations" are sanitary and facility related. It is surprising that even though we are analysing restaurants, there is only one food related violation among the top 10 and it is on the 10th place.

Even though after June 2018. violations changed, since we group violations in the same 5 categories as before, let's check if there is any change in the most frequent violation categories in failed inspections.

{% include violations_after_change.html %}

We already mentioned the big differences and stricter criteria after the change date. That can also be noticed in the most often violations. Now for the first time we can see a big impact of **violations related to employees training**. Apparently, many restaurants did not have time do adjust properly to the new regulations and failed in the past year and a half due to inadequate management or lack of allergen training for the staff. Also, the City of Chicago requires defined procedures for many situations that can happen in a restaurant, and one that is the most problematic for restaurants to comply with is **reacting in case of customer sickness.** So be careful and remind yourself of your first aid knowledge, because the restaurant employees apparently cannot help you properly. Of course, when looking at these reasons, we have to take into account that these changed violation list is valid for less than 18 months, so there is a smaller amount of inspections which can be analyzed.

Great, now we had a deeper look into the meaning of the violations and understood what are the main problems for the restaurant. However, we still haven't considered the severity of the discovered violations. Validations before change were divided into 3 groups based on how serious the violation is: critical, serious or minor violation, sorted in decreasing severity.

Of course the main interest point for us are critical violations. Those are the most serious and dangerous violations a restaurant can make. Therefore, we want to see what are the most frequent critical violations in inspections.

{% include critical_violations_before.html %}

If based on previous plots we wondered where are the food related violations in restaurants' inspections, here they are. When looking at critical violations only, the top violation is related to **proper food storage temperature**. According to the full violation description, it means that potentially hazardous food meets temperature requirements during storage, preparation and service. On second and third place we have facility related violations, and then come the ones related to adequate cleaning of all necessary areas and equipment.

All inspections end with a verdict: Pass, Pass with conditions or Fail. Hence, the percentage of restaurants having succesfully passed their last inspection in each area
is a good indicator of safety. Let's see which areas have the best and worst passing rates, and how these results evolve over time: 

<div class="map">
{% include pass_rates_year.html %}
</div>

Starting in 2010, pass rates look good: Most areas have more than 85% of their restaurants passing inspections. Things get worse in 2012, with areas like Riverdale getting as low as 50%. Fast-forward to 2019, and we can see that although pass rates seem to have improved since 2012, they are still worse than in 2010. From 2010 to 2019, areas have overall seen their percentage of restaurants deemed safe by the authorities decrease, with Austin going from 84.8% to 74.8%, and Hermosa going from 94.1% to 75%

Since inspections have gotten stricter over time, has the overall pass rate of Chicago restaurants decreased? Not significantly.

{% include pass_year.html %}

From 74% in 2010 to 65%, the inspection pass rate of Chicago has decreased, but by less than 10%.  

<h2>Restaurant Chains </h2>

Now that you know which areas are the safest to eat in, you are probably choosing a restaurant. Like most people, you might want to stick with famous restaurant chains, such as McDonald's, Burger King or KFC. But which of these chains is actually the safest? We have computed and compared some metrics to help you make the "healthiest" choice, starting with the number of violations over time:

{% include chain_violation_year.html %}

From 2010 to 2019, the ranking of the safest restaurant chains changes every year: Subway starts as the safest, quickly replaced by Starbucks, then Taco Bell, KFC and finally Taco Bell again.  
Violation numbers advise us to prefer tacos (Taco Bell) to fried chicken (KFC). What about inspection pass rates?

{% include chain_pass_rate.html %}

While pass rates are quite close for all chains, Starbucks pulls ahead of the competition with 88% of its inspections being succesful.  
So, if you're visiting Chicago and you want some fast and clean food, Starbucks is the way to go... Unless you want a proper meal instead of a frappuccino, in which case Taco Bell can be a good choice.


<h2> Safe and Dangerous Areas to Eat </h2>


Now that we have discovered what are the violations restaurants make, let's use these insights to get some recommendations which places should be favored, and which ones avoided. Our idea is to use area in which the restaurant is located and try to discover which community areas are the safest ones considering the inspections' results. 

As we previously discussed, not all violations are the same, and the most severe ones are critical violations. Therefore, if there are many inspections in which they were discovered, the area is considered more dangerous.

<div class="map">
{% include percentage_critical_violations.html %}
</div>

By looking at districts, we can see that South Side restaurants make the most critical violations, where Northwest side is the safest district considering this criteria. If we take a closer look, Washington park restaurants on average make critical violation every fourth inspection. Oakland in South Side and Uptown in Far North Side are also highly ranked and can be qualified as dangerous areas. 


Another criteria we can use is number of inspections ended in failure. Naturally, if there are more failures, the area is more dangerous. We will check the proportion of failures among total inspections for each area. If it is higher for some areas than for others, we consider them more dangerous areas.

<div class="map">
{% include percentage_failed_inspections.html %}
</div>

Again, South Side is the district to avoid. Interestingly, city center has the lowest percentage of failed inspections. As the critical areas, we again have Oakland and Washington park as leading restaurant-dangerous communities in Chicago.

Even though we are now focused on choosing the place to eat based on formal criteria like inspections, we would still like to include some user's opinion. Among all the inspections, there are some inspections of type complaint and suspected food poisoning. Complaint type means that the inspection is done in response to a complaint against the establishment. Suspected food poisioning is a specific type of complaint based inspection, when the inspection is done in response to one or more persons claiming to have gotten ill as a result of eating at the establishment. Therefore we will check number of inspections of type Complaint or Suspected Food Poisoning, because these inspections imply that users haven't been satisfied with the conditions in the restaurants. 

<div class="map">
{% include average_number_of_complaints.html %}
</div>

We can clearly see that South is deffinitely more problematic than North. Chatham and Calumet Heights have on average around 3.5 complaints per restaurant. What is really interesting that Oakland, previously considered as food-dangerous area, didn't have any complaint inspection in the previous ten years! Apparently users do not notice the problems that inspectors continuously find in the Oakland restaurants.

Maybe in the previous parts we focused on the negative events for restaurants. Everyone makes mistakes from time to time, and that is why we wanted to measure overall performance of the areas. Apart from considering how many times restaurants failed, we will now consider all of the results they could achieve. We increase the score if  a restaurant passed with conditions, and increase even more if it was a clear passing. Of course, we penalize very hard the worst result which signifies that the restaurant has been closed due to very poor performance on inspection.  Let's see which result this holistic approach gave.

<div class="map">
{% include safety_scores_by_community_areas_map.html %}
</div>

According to the safety scores we calculated, it seems that the safest places to eat are the the two Airports (O'Hare, Garfield Ridge, Clearing) and the City Center (Loop). That's right, you may need to pay extra, since those are central tourist points, but you can be quite sure that the restaurant you are eating is fulfilling the regulations. As you are going further from this places, the risk increases. If you end up in Far South Side, feel free to grab something at South Deering, but be careful not to get lost in the streets, you may end up in Riverdale which has the worst overall restaurant performance in inspections.
    
<h2>Are you ready for your trip?</h2>
  
We have dived deep into Chicago's sanitary inspections! Having seen all these maps, you probably now know Windy City like the back of your hand. Generally, we have seen that Chicago restaurants mostly fullfil sanitary safety requirements, but there are still things you should watch out for. Firstly it would be good to think twice before grabbing lunch in South or Far South Chicago. Those areas seem to have largest amount of food poisoning cases. 
   
Although South has also the greatest number of violations, it might still be worth a try. **As we all know, everything that is risky definitely tastes better.** We've seen that generally restaurants in Chicago pass the sanitary inspections, most of them also managed to fix commited violations and improve their standards. This definitely gives us some hope for a high-end culinary experience in one of the most populous cities in the USA. If you are a fast-food fan and plan to enjoy a real american meal, we would suggest to keep away from KFC for some time in the next year, their violation trend is increasingly distressing...   
  
Finally if you have always dreamt of basing your own food business we definitely recommend to focus on keeping your floor clean and your refrigerator cold. 




