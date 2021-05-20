
 # Covid 19 contact tracing

Covid 19 contact tracing

## Summary

DBSCAN is a density-based data clustering algorithm that groups data points in a given space. The DBSCAN algorithm groups data points close to each other and marks outlier data points as noise. I will use the DBSCAN algorithm for the task of contact tracing with Machine Learning.
The dataset that I will use in this task is a JSON data which can be easily downloaded from here : https://raw.githubusercontent.com/amankharwal/Website-data/master/livedata.json


## Background

Contact tracing is a process used by public health ministries to help stop the spread of infectious disease, such as COVID-19, within a community. here, I will take you through the task of contact tracing with Machine Learning.


## How is it used?

Once a person is positive for coronavirus, it is very important to identify other people who may have been infected by the patients diagnosed. To identify infected people, the authorities follow the activity of patients diagnosed in the last 14 days. This process is called contact tracking. Depending on the country and the local authority, the search for contacts is carried out either by manual methods or by numerical methods.

Here, I will be proposing a digital contact tracing algorithm that relies on GPS data, which can be used in contact tracing with machine learning.

CODE:
# import libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
import datetime as dt
from sklearn.cluster import DBSCAN
df = pd.read_json(‘livedata.json’)
df.head()

# Now let’s create a model for contact tracing using the DBSCAN model
def get_infected_names(input_name):

    epsilon = 0.0018288 # a radial distance of 6 feet in kilometers
    model = DBSCAN(eps=epsilon, min_samples=2, metric='haversine').fit(df[['latitude', 'longitude']])
    df['cluster'] = model.labels_.tolist()

    input_name_clusters = []
    for i in range(len(df)):
        if df['id'][i] == input_name:
            if df['cluster'][i] in input_name_clusters:
                pass
            else:
                input_name_clusters.append(df['cluster'][i])
    
    infected_names = []
    for cluster in input_name_clusters:
        if cluster != -1:
            ids_in_cluster = df.loc[df['cluster'] == cluster, 'id']
            for i in range(len(ids_in_cluster)):
                member_id = ids_in_cluster.iloc[i]
                if (member_id not in infected_names) and (member_id != input_name):
                    infected_names.append(member_id)
                else:
                    pass
    return infected_names
     
## Now, let’s generate clusters using our model:

abels = model.labels_
fig = plt.figure(figsize=(12,10))
sns.scatterplot(df['latitude'], df['longitude'], hue = ['cluster-{}'.format(x) for x in labels])
plt.legend(bbox_to_anchor = [1, 1])
plt.show()    

## To find people who may be infected by the patient, we’ll just call the get_infected_names function and enter a name from the dataset as a parameter:
ex : print(get_infected_names("Erin")

## this must print [ivan]

## Data sources and AI methods
 https://raw.githubusercontent.com/amankharwal/Website-
