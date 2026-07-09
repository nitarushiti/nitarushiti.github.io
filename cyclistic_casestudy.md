## Cyclistic Bike Share Case Study 

### Project overview
This project was completed as part of the Google Data Analytics Professional Certificate program. The objective was to analyze historical trip data from Cyclistic, a fictional bike-sharing company, to identify behavioral differences between casual riders and annual members and develop recommendations to increase membership conversions.

### Objectives
The company currently offers flexible pricing plans with single-ride passes, full-day passes, and annual memberships. Customers who purchase single or full-day passes are referred to as casual riders, and those who purchase annual memberships are Cyclistic members. The goal of this analysis was to understand how casual riders and members use the service differently and identify opportunities to convert casual riders into long-term members.

### Methodology
Data from 2019–2020 was cleaned and standardized to ensure consistent analysis. Date and time fields were converted to datetime format for further calculations. Ride duration was calculated o compare average trip lengths between members and casual riders. A new column was created to store day of the week for each trip to examine weekly trip patterns between the two groups. 


```python
Create new column to store ride length data
rides_total[["ride_length"]]= np.nan

Calculate ride length based on start and end times for each ride
rides_total['ride_length'] = (rides_total.end_time - rides_total.start_time) / pd.Timedelta(seconds=1)
rides_total['ride_length'] = (rides_total['ride_length'] / 60).round(2)

Create new column to store day of the week
rides_total["day_of_week"] = np.nan
rides_total["day_of_week"] = rides_total["end_time"].dt.day_name()

Comparing weekly ridership data by user type
weekly_count = rides_total.groupby(['usertype', 'day_of_week']).agg(count_rides=('ride_length', 'count'))
weekly_count  = weekly_count.reset_index()

Visualizing weekly patterns 
user_plot2 = sns.barplot(x="day_of_week",
           y="count_rides",
           hue="usertype", palette = 'Purples',
           data=weekly_count)
user_plot2.set(xlabel='Day of Week ', ylabel='Number of Rides')
user_plot2.set(title = ' Number of Rides by User Type ')
user_plot2.get_legend().set_title("")
plt.savefig('week_rides_usertype.png', bbox_inches = "tight")
plt.show()
```

### Key Findings
- Members accounted for approximately 91% of all rides, while casual riders accounted for 9%.
- Members averaged approximately 13 minutes per ride.
- Casual riders averaged approximately 85 minutes per ride.
- Members rode most frequently during weekdays.
- Casual riders showed relatively stronger weekend activity.
- Both groups have their longest rides around 2 pm, but members show a much stronger morning peak.

*Table 1. Quick summary of ridership data*
| User type | Ride count | Average trip duration | Most popular day | 
| Member    | 720313     | 13 minutes            | Tuesday          |
| Casual    | 71643      | 85 minutes            | Sunday           |

<img src="images/p2_usertypes.png?raw=true"/>

*Figure 1 Distribution of user types for Cyclistc's bikes during 2019-2022*

<img src="images/p2_weeklyrides.png?raw=true"/>

*Figure 2. Total number of rides taken by each user type (a), average trip duration for each user type during the week (b).	

### Insights
-	**Members drive most usage**

Most rides were taken by annual members. This suggests that memberships are highly effective at encouraging repeat usage and represent the company's core customer base.
-	**Casual riders take longer trips** 

Casual riders consistently recorded significantly longer trip durations than members. This suggests that casual riders primarily use the service for leisure, recreation, sightseeing, or other non-routine activities, while members use the service as part of their regular transportation habits.
-	**Different weekly usage patterns**

Ride volume remained high for members throughout the work week before dropping on weekends. Casual riders represented a larger share of weekend activity. These patterns suggest that members are primarily commuters while casual riders are more likely to ride for recreational purposes.

### Conclusion
The analysis revealed clear behavioural differences between casual riders and members. Members use Cyclistic as part of their daily transportation routines, while casual riders are more likely to use the service for longer recreational trips. By focusing marketing efforts on frequent casual riders and emphasizing the value of membership, Cyclistic can increase membership adoption and improve long-term customer retention.

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
