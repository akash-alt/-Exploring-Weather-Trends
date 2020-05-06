# Exploring Weather Trends
In this project, I have analyzed local and global temperature data and compared the temperature trends where I live to overall global temperature trends.


# Table of contents
1. Installation
2. Database
3. File Descriptions
4. Analyzing Data
5. Observations


# Installation

The following libraries and tools are used in this project:
.  SQL
.  Pandas
.  Matplotlib
.  The code should run with no issues using Python version 3.*.

# Database

Database used to load temperature data has following database schema:

.It has 3 tables
   
    city_list- This contains a list of cities and countries in the database.
    city_data - This contains the average temperatures for each city by year (ºC).
    global_data - This contains the average global temperatures by year (ºC).
    
Following SQL statements are used to load the data from database:
   
 . Loading local_temp_data
   
     SELECT *
     FROM city_data
     WHERE city = 'Hyderabad' AND country = 'India';
     
. Loading global_temp_data 
     
     SELECT *
     FROM global_data;
     
. The data loaded is then exported as .CSV


# File Descriptions

The following files are loaded from the SQL database:

   . local_temp_data.csv: Holds average temperature data for the local or nearest city to me.
   . global_temp_data.csv: Holds average temperature data for global.
   . ExploringWeatherTrends.ipynb: Code written in python for analyzing temperature trends.
   
   
# Analyzing Data

Following steps are performed in notebook while analyzing exported data from SQL database:
   
   Intially, data for both local and global temperature trends are loaded and displayed.

      local_temp_data = pd.read_csv('local_temp_data.csv')
      global_temp_data = pd.read_csv('global_temp_data.csv')
      
      
Then I have done some pre-processing to remove certain years from years column of global temperature data to match the years column of local temperature data so that the results will be consistent.

    global_temp_data.drop(index = [x for x in range(46)], axis = 0, inplace = True)
    global_temp_data.drop(index = [264, 265], axis = 0, inplace = True)
    global_temp_data.index = local_temp_data.index     
    


Then visualized the avg_temp per year for both local and global temperatures in one plot using matplotlib's plot() function.
   
   
    plt.plot(global_temp_data['year'], global_temp_data['avg_temp'], label = 'global_temp_trend')
    plt.plot(local_temp_data['year'], local_temp_data['avg_temp'], label = 'local_temp_trend')
    plt.xlabel('Year')
    plt.ylabel('Average_temp')
    plt.title('Global vs Local Temp Trends')
    plt.legend()
    plt.show()
    
    
 # Calculating Moving Averages 
 
   . From the plot it can be observed that there are lot of spikes when we plot temperature trends for avg_temp per year and this makes difficult to analyze temperature trends. Hence we go for moving averages.

   . Moving averages are calculated in the following way using pandas rolling() function which takes window as an argument and then taking the average using mean() method.
        
          local_temp_data['moving_avg'] = local_temp_data['avg_temp'].rolling(10).mean()
          global_temp_data['moving_avg'] = global_temp_data['avg_temp'].rolling(10).mean()
          
 Note: While calculating moving_avg itself I have created seperate column called moving_avg for both local and global temperature data.

   . After calculating moving_avg, again local and global temperature trends are visualized using matplotlib's plot() function but this time for moving_avg which is calculated for 10 years window size. The following visual shows this:         
    
    
    plt.plot(global_temp_data['year'], global_temp_data['moving_avg'], label = 'global_temp_trend')
    plt.plot(local_temp_data['year'], local_temp_data['moving_avg'], label = 'local_temp_trend')
    plt.xlabel('Year')
    plt.ylabel('Moving average')
    plt.title('Global vs Local Temp Trend')
    plt.legend()
    plt.show()
    
    
  . Now you can observe from the plot that there are very less spikes(almost no spikes) and curves look smooth which hepls us analyzing temperature trends much easier.

  . I have also calculated difference in moving averages for local and global temperature data to draw additional conclusions.  

      
      global_temp_data['diff_moving_avg'] = local_temp_data['moving_avg'] - global_temp_data['moving_avg']
    

# Observations

 1. From the following visual we can make following conclusions:
  
    On average, temperature for local(Hyderabad) is higher compared to the global, which means the city I live in is hotter compared to the global.

2. And this trend is consistent over time which can be seen for years 1800s - 2000s
At first glance from the green line in above visual, which indicates the difference in moving average for local and global temperature shows that the rate of change of temperature for Hyderabad compared with global is almost same for all years.
     
3. Looking at the overall trend it looks like world is getting hotter because the curves keep on rising as years progressing.

   And this trend is consistent for the last few hundred years.
4. Looking at the visual it is found that most of the countries are cooler as compared to where I live, that is because the region I live comes under the tropical region which occupies small region on Earth as compared.
