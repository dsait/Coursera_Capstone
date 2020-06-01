# Introduction

This is a summary blogpost of the full [report](https://github.com/dsait/Coursera_Capstone/blob/master/Report.md)

You can find more details too in the [notebook](https://github.com/dsait/Coursera_Capstone/blob/master/The%20Battle%20of%20Neighborhoods.ipynb)

The goal of this project is to find out if there is any link between the number and price of Airbnb accommodations listed in Paris's neighborhoods and the venues (restaurants, museums, etc.) that are located in those neighborhoods.

# Data

The project leverages data from 3 sources:
- PARIS Data, which is the official Open Data website of the city of Paris.
- Airbnb listings in the city.
- Foursquare API, particularly the /venues/explore/ endpoint which returns a list of recommended venues in the vicinity of a specific neighborhood.

# Analysis

I start by plotting the neighborhoods on a map:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-neighborhoods.png"/>

Then I pull the venues from Foursquare and keep only the 10 most common venues per neighborhood:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/most-common-venues-per-neighborhood.png"/>

Using the KMeans algorithm with 5 (the optimal) clusters, I segment the neighborhoods and draw them on a map:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-clustered-neighborhoods.png"/>

Then I pull and clean the listings from Airbnb, group them into 2 price categories (high: black, low: white), and plot them on the same map:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-on-map.png"/>

# Observations

Most of the neighborhoods belong to clusters 0 (yellow), 1 (red) and 2 (purple).

Most of the neighborhoods of Cluster 2 (purple) are in the East of the city.

All the neighborhoods of Cluster 0 (yellow) and most of the neighborhoods of Cluster 1 (red) are in the West of the city.

All neighborhoods of Cluster 3 (grey) and Cluster 5 (green) are peripheral.

Although I segmented the neighborhoods based solely on the venues category, there is an implicit geographical dimension to it: similar neighborhoods (venue wise) are located next to each other.

This is somehow expected as the West of Paris is generally more affluent than the East.

Here is an example of housing prices per square meter published by this newspaper [page](https://immobilier.lefigaro.fr/article/a-paris-un-marche-immobilier-a-trois-vitesses-entre-est-ouest-et-centre_2fa31cf0-11e6-11ea-8bf6-37a3558e7c02/) on December 2019:

<img src="https://i.f1g.fr/media/eidos/805x453_crop/2019/11/29/XVM77fe283e-11f8-11ea-8bf6-37a3558e7c02-805x453.jpg"/>

Some clusters contain more accommodations than others, but all of them contain accommodations in both the high and low price ranges:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-bar-chart.png"/>

Clusters 3 & 4 host very few listings, which makes sense, because they are peripheral and have very few interesting venues.

Clusters 0, 1 and 2 host increasing number of listings, cluster 2 being the top, but the prices are almost evenly distributed between High and Low.

Here is a zoomed version of the map where high (black) and low (white) price accommodations are randomly scattered across clusters:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-on-map-zoomed.png"/>

# Conclusion

We can therefore safely conclude that:
- Depending on where you want to stay, you will find many or few available listings, the further you go from the city center, the fewer they are
- Everywhere you go in the city, you’ll find both high and low price accommodations. Thus the price category does not depend on the location.

This analysis has surely many shortcomings.

I can already think of two.

The Airbnb listings were extracted on 15 April 2020, during the Covid-19 lockdown, so the prices and numbers of accommodations might not reflect normal circumstances.

Because I use a radius of 500 meters to search for venues, some venues are considered as if they belong to more than one neighborhood.

Here is an example of this French restaurant that is considered as part of 2 neighboring neighborhoods, which oddly belong to 2 different clusters:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/duplicate-venues.png"/>

This can be fixed by using the shapely package to check if the coordinates of the venue are inside the neighborhood's polygon
