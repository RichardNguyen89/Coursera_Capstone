This notebook is an implementation of the IBM Data Science Certificate Capstone project.
Author: Linh Nguyen
Battle of the Neighborhoods
INTRODUCTION
PROJECT MOTIVATION
As of 07/28/2020, the U.S. has 4.2 millions confirmed cases of COVID-19. New York City is the at the epicenter of the battle against the pandemics with more than 221,000 cases confirmed and approximately 56,000 cases of hospitalization.

This project aims to analyze the hospital bed density and the COVID-19 case rate for each neighborhood in the New York City. The hospital bed density is measured as the number of beds per 1,000 people. It can be considered as one of the measurement of the service availbility. When pitted against the COVID-19 case rate, it can provide a better picture of the pandemic.

This project is also motivated by lisu1222: The battle of the neighborhoods: hospital density

and ruddra: capstone project

Data
NYC Neighborhood Data is a json file dowloaded from New York City Neighborhoods Names. It includes latitude and longitude for each neighborhood of the 5 boroughs in NYC.

NYC Population Data includes population data per neighborhood that were scrapped from Wikipedia, and then integrated with New York Neighborhood Data.

Hospital Data is scrapped from New York State Department of Health. It contains the name, bed type and the corresponding bed numbers in each type.

NYC COVID 19 case rate data is taken from NYC health. The data set contains case count, case rates, death rate and so on. These rate are calculated per 100,000 and thus will need to be processed.

These data sets will be processed, explored and integrated for further analysis.

METHOD
1. NYC Neighborhood and Population Data
The data contained in New York City Neighborhoods Names is in a json file. The latitude and longitude for each neighborhood of the 5 boroughs in NYC is extracted and saved into a dataframe. Beauty Soup was used to scrapped the wiki pages of each neighborhood to find the population data. These 2 datasets are then combined together as follow:

2. Hospital Data
I used Foursquare API to explore the information of the hospital in each neighborhood, including name of the hospital, neighborhood, borough, longitude and latitude.
With ID of each hospital, I then used Beauty Soup to scrap the New York State Department of Health websites to obtain information on the name of the hospital, bed type and the corresponding bed numbers in each type.\ The package fuzzywuzzy was then used to match the information of the hospital in the dataset 1 and the dataset 2 on the name of the hospital.
After processed, this dataset is merged with the neighborhood and population data set. Then the ICU bed per Hundred people and bed per hundred people were added.

3. Covid case rate Data
NYC COVID 19 case rate data is taken from NYC health. The data set contains case count, case rates, death rate and so on.



I decided to limit the relevant information to the case rates only.
This dataset is then integrated into the hospital data set to obtain the nyc final data set by matching on the neighborhood variable.
(At this stage, I applied a rather greedy matching process which result in duplicated data. This will be discussed as a limitation of the project)

4. Neighborhood Clustering Analysis
We will use K-means to cluster NYC neighborhoods.

First we select features for clustering: ICU Bed Per Hundred People and Bed Per Hundred People and COVID_CASE_RATE (per hundred). Data such as Population is already included in other variables and thus, I chose to leave it out. We then normalize the data since K-Means algorithm requires standardized dataset to calculate distances.

Then we use elbow method to find the optimum number of clusters and the output optimum number of k is 7.

Examine clusters















Visualization of clusters

Finally, I visualized the clusters on the New York map with the radius corresponding to the Covid_case_rate of each neighborhood.

Discussion
Looking into the results, we find that the algorithm classifies the neighborhoods into 7 clusters with extensive logics as follow:

ICU	Bed	COVID rate
1	low	low	low
2	high	high	low
3	low	medium	medium
4	low	high	low
5	low	high	high
6	low	low	high
7	low	high	medium
However, even though the rate of group 1 is low, the rate of beds over the rate of cases (less than 1 in most cases) may indicate that group 1 can not meet the demand at some points. Thus, group 2, group 4 and group 7 may provide better service availability. Among them, group 2, Murray Hill of Manhattan has the highest availability.

Limitations

The hospital beds data we collected from New York State Department of Health may not include the latest information.

The NYC population data we collected from Wikipedia pages are from 2010, that is not very accurate.

The COVID dataset initially contains an extensive neighborhood data. During the process, I essentially reduced this information to match with the neighborhood data from the hospital dataset using a strict approach. Some data was lost at this point. If there is a way to preserve this data, we can conduct a more extensive clustering experiment.

In this project, I used K-mean clustering. The result was reasonable. However, there can be other methods to cluster the dataset that worth trying.

Conclusion
This project aims at clustering the neighborhood in New York city based on the data of the ICU and hospital beds and COVID rate case of the neighborhoods. To do so, collecting, cleaning, transforming and processing the data was necessary. In the end, we identify 7 clusters of neighborhoods.