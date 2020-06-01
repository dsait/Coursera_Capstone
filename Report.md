# Introduction/Business Problem
Say you’re planning to visit Paris soon for work or leisure and you want to stay in an Airbnb.

There are already a wide range of questions you’re asking yourself, such as in which neighborhood I should stay?

This decision depends on many factors, like the purpose of your trip, your budget and preferences.

But in all cases, it would most likely be based on these two variables:

The number and type of the venues that you deem interesting (like museums, restaurants, etc.) and that are close or easily accessible from the neighborhood where you’re staying, and

The price of the accommodation, especially if you’re on limited budget, given that Paris is definitely not the cheapest city in the world

One might be wondering for example whether or not popular neighborhoods with trending venues are always located in the center of the city?

Or is it true that all Airbnb accommodations are expensive in those neighborhoods?

Are there neighborhoods that have more Airbnb listings than others? If so, where are they located and how much they cost?

On another hand, let’s say that you have already visited Paris in the past, and you enjoyed your stay in a specific neighborhood.

This time, you want to stay in a new neighborhood, since you like novelty.

But still, you also want the new neighborhood to be similar and not too different from the previous one, because you already had a lot of fun there last time.

This project attempts to find answers to these questions, that every visitor to Paris is or should be asking themselves to make the most of their trip, and here is how it will be done.

# Data
The answer will be data-driven.

It leverages three data sources.

## PARIS Data

<img src="https://opendata.paris.fr/assets/theme_image/header.png"/>

This is the official Open Data website of the city of Paris.

It contains many datasets across many themes.

The project uses the neighborhood’s one from the urbanism section.

This is the link to the dataset and the link to the metadata.

I keep only the attributes that are relevant for the analysis, which after wrangling look like this:

The first three columns are self-explanatory, the fourth contains the geographical coordinates of the polygon representing the area of the neighborhood.

Paris has 20 boroughs (Arrondissements) and 80 neighborhoods (Quartiers). Each Arrondissement has 4 Quartiers.

## Airbnb

All Airbnb accommodation listings in Paris.

This is the link to the dataset.

It has nearly 60K records.

The columns are evident but I keep only those that are relevant for the analysis.

Here is how it looks like after cleaning:

Every row represents an accommodation, and it contains information about the neighborhood where it is located, its geographical coordinates, type, price and the number of reviews it got.

## Foursquare

I use the /venues/explore/ endpoint of the Foursquare API to get a list of recommended venues in the vicinity of a specific neighborhood.

The details of the request and response attributes of this endpoint can be found in this official documentation page.

After cleaning, the data looks like this:

Every row gives the name of the venue, its geographical coordinates, name, type and the neighborhood where it is located.

5066 venues were pulled under 293 unique venue category.
