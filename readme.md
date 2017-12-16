import pandas as pd 
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sbrn
import matplotlib.patches as mpatches

CD = pd.read_csv("Desktop\\city_data.csv")
DD = pd.read_csv("Desktop\\ride_data.csv")

city_type = CD["type"]

city_ride_df = pd.merge(CD, DD, how='outer', on='city', sort=True)
city_ride_df.head()

urban = city_ride_df[city_ride_df["type"]== "Urban"]
rural = (city_ride_df[city_ride_df["type"]== "Rural"])
suburban = (city_ride_df[city_ride_df["type"]== "Suburban"])

urban_average = urban.groupby("city")["fare"].mean()
urban_count = urban.groupby("city")["fare"].count()
urban_size = urban.groupby("driver_count").size()

suburban_average = suburban.groupby("city")["fare"].mean()
suburban_count = suburban.groupby("city")["fare"].count()
suburban_size = suburban.groupby("driver_count").size()

rural_average = rural.groupby("city")["fare"].mean()
rural_count = rural.groupby("city")["fare"].count()
rural_size = rural.groupby("driver_count").size()

sbrn.set()
plt.figure(figsize=(12,10))

Urban_x = urban_count
Urban_y = urban_average

Suburban_x = suburban_count
Suburban_y = suburban_average

Rural_x = rural_count
Rural_y = rural_average

blue_bubs = mpatches.Patch(color = "cyan", label="Suburban")
yellow_bubs = mpatches.Patch(color = "gold", label="Rural")
red_bubs = mpatches.Patch(color='coral', label='Urban')
plt.legend(handles=[red_bubs, yellow_bubs, blue_bubs])

plt.title("Pyber Ride sharing")
plt.xlabel("Total Rides per City")
plt.ylabel("Average fare Price per City")
 
plt.scatter(Urban_x, Urban_y, color = "coral",s = urban_size*10, cmap="Blues", alpha=0.5, edgecolors="black", linewidth=2)
plt.scatter(Rural_x, Rural_y, s = rural_size*10,color = "gold",cmap="Blues", alpha=0.5, edgecolors="black", linewidth=2)
plt.scatter(Suburban_x, Suburban_y, s = suburban_size*10 ,color = "cyan",cmap="Blues", alpha=0.5, edgecolors="black", linewidth=2 )
plt.show()

sbrn.set()
plt.title("Percentage of Total Rides by City Type")
lables = ["Rural", "Suburban", "Urban"]
explode = (0, 0, 0.5)
colors = ["gold", "cyan", "coral" ]

perecent_Fares = pd.DataFrame({"Perecentage":round(city_ride_df.groupby("type")["fare"].count()/len(city_ride_df["fare"])*100, 2)})

plt.pie(perecent_Fares, labels = lables, autopct = "%1.1f%%", shadow=True, explode = explode, colors = colors )
plt.axis("equal")
plt.show()

total_fare = pd.DataFrame({"Perecentage of Total Fares":round(city_ride_df.groupby("type")["fare"].sum()/sum(city_ride_df["fare"])*100, 2)})

sbrn.set()
plt.title("Percentage of Total Fares by City Type")
lables = ["Rural", "Suburban", "Urban"]
explode = (0, 0, .5)
plt.pie(total_fare, colors = colors, autopct = "%1.1f%%",startangle = 100, labels = lables ,shadow = True, explode = explode)
plt.axis('equal')
plt.show()

total_drivers = round(city_ride_df.groupby("type")['driver_count'].sum()/sum(city_ride_df["driver_count"])*100, 2)

sbrn.set()
explode = (0,0,.5)
plt.pie(total_drivers, labels = lables, explode = explode, startangle = 100, shadow = True ,colors = colors, autopct = "%1.1f%%")
plt.axis("equal")
plt.title("Total Number of Drivers by City Type")
plt.show()

#Three trends
#1) The urban population generates more than half the total money because of its greater population over the other two types of areas.
#2) The rural landscapes, because of driver limits has much fewer drivers bringing prices in their greatest range of users from $30 - $37.
#3) The best margins come in the suburban areas because %31.5 is shared among %13 drivers per this type yet %86.1 of the drivers in the busy urban areas are fighting for %62 of the money to go around.
