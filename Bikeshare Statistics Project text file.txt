import time
import pandas as pd
import numpy as np

CITY_DATA = { 'Ch': 'chicago.csv',
              'Nyc': 'new_york_city.csv',
              'W': 'washington.csv' }

#this function to get a clear information form the user
def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    #this step to get a valid input for the cities from customer
    while True:
        city=input('please chose a city form below: \n(Ch) for chicago\n(Nyc) for new york city \n(W) for washington:\n  ').title()
        if city not in CITY_DATA:
            print("oops!: pleae enter a valed city. ")
        else:
            break

    # TO DO: get user input for month (all, january, february, ... , june)
    months=['January','February','March','April','May','June','All']
    #this step to get a valid input for the months from the customer
    while True:
        month=input('please enter all for all months or chose your month from the following:\n january\n february\n march\n april\n may\n june\n ').title()
        if month !='All' and month not in months:
                print("oops!: pleae enter a valed Month. ")
        else:
            break

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days=['Sunday',"Monday","Tuseday","Wednesday","Thursday","Friday",'All']
   #this step to ger a clear information about days from customer
    while True:
        day=input("please enter all for all Days or chose your Day from the following:\n Sunday\n Monday\n Tuseday\n Wednesday\n Thursday\n Friday\n").title()
        if day != 'All' and day not in days:
            print("oops!: pleae enter a valed Day.")
        else:
            break

    print('-'*40)
    return city, month, day

#this function to start get the right data from the CITY_DATA it takes city, month and day from the previous function
def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df=pd.read_csv(CITY_DATA[city])   #to select the right city sheet
    df['Start Time']=pd.to_datetime(df['Start Time'])   #this step to convert the Start Time column to datetime format
    df['month']=df['Start Time'].dt.month     #extracting month name from the Start Time column and place it in a new column named month
    df['day']=df['Start Time'].dt.day_name()  #extracting day name from the Start Time column and place it in a new column named day

    #the below section to if we have a filter with month or with day and
    months=['January','February','March','April','May','June','All']
    if month !='All':
        month=months.index(month)+1
        df=df[df['month']==month]
    if day !='All':
        df=df[df['day']==day.title()]
    return df

#this section to calculate a results related to most common month, day and hour
def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    most_comm_month=df['month'].value_counts().idxmax()    #calculating the most common month
    print('the most common month is: {}'.format(most_comm_month))

    # TO DO: display the most common day of week
    most_comm_day=df['day'].value_counts().idxmax()   #calculating the most common day
    print('the most common day is: {}'.format(most_comm_day))

    # TO DO: display the most common start hour
    df["hour"]=df["Start Time"].dt.hour   #extracting hours from Start Time calumn and place it in column named hour
    most_comm_hour=df['hour'].value_counts().idxmax()    #calculating the moct common hour
    print("the most common start hour is: {}".format(most_comm_hour))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

#thid section to calculate a results related to most common stations
def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    most_comm_start_st=df['Start Station'].value_counts().idxmax()    #calculating the most common start station
    print("the most common start station is: {}".format(most_comm_start_st))

    # TO DO: display most commonly used end station
    most_comm_end_st=df['End Station'].value_counts().idxmax()    #calculating the most common end station
    print('the most common end station is: {}'.format(most_comm_end_st))

    # TO DO: display most frequent combination of start station and end station trip
    df['Start and End stations']=df['Start Station']+"...to..."+df['End Station']  #making a compination coumn between start station and end station
    most_common_stations=df['Start and End stations'].value_counts().idxmax()    #calculating the most common trip
    print('the most common start and end stations is: {}'.format(most_common_stations) )

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

#this section to calculate a result related to trip durations
def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_travel_time=df['Trip Duration'].sum()    #calculating the total travels time in seconds and hours
    print('the total travel time is: ',total_travel_time," in second and: ",total_travel_time/3600," in hours." )

    # TO DO: display mean travel time
    mean_travel_time=df['Trip Duration'].mean() #calculating the avg travels time in seconds and hours
    print('the mean travel time is: ',mean_travel_time," in second and: ",mean_travel_time/3600," in hours." )


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

#this section to calculate a result related to users
def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    counts_of_user_type=df['User Type'].value_counts() #this line groups and sum customers by type
    print('the counts of user type is:\n{}'.format(counts_of_user_type))

    # TO DO: Display counts of gender
    if 'Gender' in df:
        counts_of_user_gener=df['Gender'].value_counts() #this line groups and sum customers by gender
        print('the counts of user Genders is:\n{}'.format(counts_of_user_gener))

    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' in df:
        print('the earliest year of bith is: ',df['Birth Year'].min())   #the earliest bithyear
        print('the most recent year of bith is: ',df['Birth Year'].max())    #the most recent bithyear
        print('the most common year of bith is: ',df['Birth Year'].mode()) #the most common birthyear
        print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

#this section to present the 5 rows of data for the user if he/she need
def show_data(df):
    #the below section to get a clear information from the customer
    i=0
    answer=['yes','no']
    while True:
        show=input('do you want to see 5 rows of data? ( yes / no )')
        if show =='yes':
            print(df[i:i+5])
            i+=5
            continue
        elif show=='no':
            print('thanks')
            break
        else:
            print('i didn\'t understand your chooise!, please try again. ')
    print('-'*40)


def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
        show_data(df)
        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            print("See you later")
            break



if __name__ == "__main__":
	main()
