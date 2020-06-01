# Introduction/Business Problem
Say you’re planning to visit Paris soon for work or leisure and you want to stay in an Airbnb.

There are already a wide range of questions you’re asking yourself, such as in which neighborhood I should stay?

This decision depends on many factors, like the purpose of your trip, your budget and preferences.

But in all cases, it would most likely be based on these two variables:

- The number and type of the venues that you deem interesting (like museums, restaurants, etc.) and that are close or easily accessible from the neighborhood where you’re staying, and
- The price of the accommodation, especially if you’re on limited budget, given that Paris is definitely not the cheapest city in the world

One might be wondering for example whether or not popular neighborhoods with trending venues are always located in the center of the city?

Or is it true that all Airbnb accommodations are expensive in those neighborhoods?

Are there neighborhoods that have more Airbnb listings than others? If so, where are they located and how much they cost?

On another hand, let’s say that you have already visited Paris in the past, and you enjoyed your stay in a specific neighborhood.

This time, you want to stay in a new neighborhood, since you like novelty.

But still, you also want the new neighborhood to be similar and not too different from the previous one, because you already had a lot of fun there last time.

This project attempts to find answers to these questions, that every visitor to Paris is or should be asking themselves to make the most out of their trip, and here is how it will be done.

# Data
The answer will be data-driven.

It leverages three data sources.

## PARIS Data

<img src="https://opendata.paris.fr/assets/theme_image/header.png"/>

This is the official Open Data website of the city of Paris.

It contains many datasets across many themes.

The project uses the [neighborhood's](https://opendata.paris.fr/explore/dataset/quartier_paris/information/) one from the urbanism section.

This is the [link](https://opendata.paris.fr/explore/dataset/quartier_paris/download/?format=csv&timezone=Europe/Berlin&lang=fr&use_labels_for_header=true&csv_separator=%3B) to the dataset and the [link](https://opendata.paris.fr/api/datasets/1.0/quartier_paris/attachments/quartier_pdf/) to the metadata.

I keep only the attributes that are relevant for the analysis, which after wrangling look like this:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-neighborhoods-df.png"/>

The first three columns are self-explanatory, the fourth contains the geographical coordinates of the polygon representing the area of the neighborhood.

Paris has 20 boroughs ([Arrondissements](https://en.wikipedia.org/wiki/Arrondissements_of_Paris)) and 80 neighborhoods ([Quartiers](https://en.wikipedia.org/wiki/Quarters_of_Paris)). Each Arrondissement has 4 Quartiers.

## Airbnb

<img height="128" with="128" src="https://news.airbnb.com/wp-content/uploads/sites/4/2017/01/airbnb_vertical_lockup_web.png"/>

All Airbnb accommodation listings in [Paris](http://insideairbnb.com/get-the-data.html).

This is the [link](http://data.insideairbnb.com/france/ile-de-france/paris/2020-04-15/visualisations/listings.csv) to the dataset.

It has nearly 60K records.

The columns are evident but I keep only those that are relevant for the analysis.

Here is how it looks like after cleaning:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-listings-df.png"/>

Every row represents an accommodation, and it contains information about the neighborhood where it is located, its geographical coordinates, type, price and the number of reviews it got.

## Foursquare

<img height="128" with="128" src="https://img.pngio.com/company-entertainment-food-foursquare-nightlife-perfect-foursquare-png-512_512.png"/>

I use the /venues/explore/ endpoint of the Foursquare API to get a list of recommended venues in the vicinity of a specific neighborhood.

The details of the request and response attributes of this endpoint can be found in this official documentation [page](https://developer.foursquare.com/docs/api-reference/venues/explore/).

After cleaning, the data looks like this:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-venues-df.png"/>

Every row gives the name of the venue, its geographical coordinates, category and the neighborhood where it is located.

5066 venues were pulled under 293 unique venue category.

# Methodology

Once the neighborhoods of Paris are pulled from the PARIS Data website, I start by plotting them on a map.

Unlike what we did in the course, I use polygons instead of markers, because they are more suitable for drawing areas:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-neighborhoods.png"/>

No area is left empty, which means that the neighborhoods cover indeed the entire surface of the city.

Then I pull the venues from Foursquare and keep only the 10 most common venues per neighborhood:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/most-common-venues-per-neighborhood.png"/>

Because this is a segmentation problem, I use the KMeans algorithm with 5 (the optimal) clusters.

Then I draw the clustered neighborhoods on a map:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-clustered-neighborhoods.png"/>

The segmentation resulted in 5 clusters:

- Cluster 0 : yellow

- Cluster 1 : red

- Cluster 2 : purple

- Cluster 3 : grey

- Cluster 5 : green

Then I pull and clean the listings from Airbnb.

The processing time is too long because there are nearly 60K records.

This is why I filter only entire apartments that received more than 50 reviews.

I group them into two price categories:

- High if the price is above the median

- Low if the price is below the median

To get in which neighborhood each accommodation is located, I take its coordinates and see if it is inside the neighborhood polygon, because the neighborhood provided in the dataset is actually the borough and not the administrative neighborhood.

This is how they look like after cleaning:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-listings-df.png"/>

Then I plot them on a map:

- the black dots belong to the high price category

- the white dots to the low price category

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-map.png"/>

No dot is outside the city, which means that the accommodations were carefully filtered and only those that are located in Paris were kept.

# Results

## Neighborhoods

The segmentation resulted in 5 clusters:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clusters-count.png"/>

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/paris-clustered-neighborhoods.png"/>

Most of the neighborhoods belong to clusters 0 (yellow), 1 (red) and 2 (purple).

Most of the neighborhoods of Cluster 2 (purple) are in the East of the city.

All the neighborhoods of Cluster 0 (yellow) and most of the neighborhoods of Cluster 1 (red) are in the West of the city.

All neighborhoods of Cluster 3 (grey) and Cluster 5 (green) are peripheral.

Although I segmented the neighborhoods based solely on the venues category, there is an implicit geographical dimension to it: similar neighborhoods (venue wise) are located next to each other.

This is somehow expected as the West of Paris is generally more wealthy than the East.

Here is an example of housing prices per square meter published by this newspaper [page](https://immobilier.lefigaro.fr/article/a-paris-un-marche-immobilier-a-trois-vitesses-entre-est-ouest-et-centre_2fa31cf0-11e6-11ea-8bf6-37a3558e7c02/) on December 2019:

<img src="https://i.f1g.fr/media/eidos/805x453_crop/2019/11/29/XVM77fe283e-11f8-11ea-8bf6-37a3558e7c02-805x453.jpg"/>

## Listings

All clusters have more low price accommodations than high price accommodations, except Cluster 0:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/listings-per-price-and-cluster.png"/>

This bar chart plots the number of accommodations per cluster and per price category:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-bar-chart.png"/>

Clusters 3 & 4 host very few listings, which makes sense, because they are peripheral and have very few interesting venues.

Clusters 0, 1 and 2 host increasing number of listings, cluster 2 being the top, but the prices are almost evenly distributed between High and Low.

It is even more apparent when it is plotted on a map
- the black dots are high price listings
- the white dots are low price listings

Both of them are evenly scattered across clusters:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-on-map.png"/>

Here is a zoomed version where high (black) and low (white) price accommodations are randomly scattered across clusters:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/clustered-listings-on-map-zoomed.png"/>

# Discussion

It is clear now that some clusters contain more Airbnb listings than others, cluster 2 being the most popular.

All clusters have both high and low price listings.

We can therefore safely conclude that:
- Depending on where you want to stay, you will find many or few available listings, the further you go from the city center, the fewer they are
- Everywhere you go in the city, you’ll find both high and low price accommodations. Thus the price category does not depend on the location.

# Conclusion

This analysis can be easily extended to other cities, simply by changing the geographical coordinates, provided they have enough listings and venues.

But it has surely many shortcomings.

I can already thing of two.

The Airbnb listings were extracted on 15 April 2020, during the Covid-19 lockdown, so the prices and numbers of accommodations might not reflect normal circumstances.

Because I use a radius of 500 meters to search for venues, some venues are considered as if they belong to more than one neighborhood.

Here is an example of this French restaurant that is considered as part of 2 neighboring neighborhoods, which oddly belong to 2 different clusters:

<img src="https://github.com/dsait/Coursera_Capstone/blob/master/duplicate-venues.png"/>

This can be fixed by using the shapely package to check if the coordinates of the venue are inside the neighborhood's polygon
