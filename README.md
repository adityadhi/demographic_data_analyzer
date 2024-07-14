# demographic_data_analyzer
This project analyzes demographic data from the U.S. Census Bureau's 1994 Adult Data Set. The analysis provides insights into various socio-economic factors such as race distribution, average age of men, education levels, and income distribution across different education levels and countries.
import pandas as pd

def demographic_data_analyzer(print_data=True):
    try:
        # Read data from the file
        df = pd.read_csv('adult.data.csv')
        
        # Display dataset information
        if print_data:
            print(df.info())
        
        # Count of different races in the dataset
        race_count = df['race'].value_counts()
        if print_data:
            print('RACE COUNT')
            print(race_count)
        
        # Count of different sexes in the dataset
        sex_count = df['sex'].value_counts()
        if print_data:
            print('SEX COUNT')
            print(sex_count)
        
        # Ages of males in the dataset
        male_ages = df.loc[df['sex'] == 'Male', 'age']
        if print_data:
            print('MALE AGES')
            print(male_ages)
        
        # Average age of men
        average_age_men = male_ages.mean()
        if print_data:
            print('AVERAGE AGE OF MEN:', average_age_men)
        
        # Percentage with Bachelors
        percentage_bachelors = (df['education'] == 'Bachelors').mean() * 100
        if print_data:
            print('PERCENTAGE WITH BACHELORS:', percentage_bachelors)
        
        # Percentage with higher education that earn >50K
        higher_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
        higher_education_rich = (df[higher_education]['salary'] == '>50K').mean() * 100
        if print_data:
            print('PERCENTAGE WITH HIGHER EDUCATION EARNING >50K:', higher_education_rich)
        
        # Percentage without higher education that earn >50K
        lower_education = ~df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
        lower_education_rich = (df[lower_education]['salary'] == '>50K').mean() * 100
        if print_data:
            print('PERCENTAGE WITHOUT HIGHER EDUCATION EARNING >50K:', lower_education_rich)
        
        # Minimum work hours per week
        min_work_hours = df['hours-per-week'].min()
        if print_data:
            print('MINIMUM WORK HOURS PER WEEK:', min_work_hours)
        
        # Percentage of rich among those who work minimum hours
        num_min_workers = df[df['hours-per-week'] == min_work_hours]
        rich_percentage = (num_min_workers['salary'] == '>50K').mean() * 100
        if print_data:
            print('PERCENTAGE OF RICH AMONG THOSE WHO WORK MINIMUM HOURS:', rich_percentage)
        
        # Country with highest percentage of rich
        country_salary = df[df['salary'] == '>50K']['native-country'].value_counts() / df['native-country'].value_counts() * 100
        highest_earning_country = country_salary.idxmax()
        highest_earning_country_percentage = country_salary.max()
        if print_data:
            print('HIGHEST EARNING COUNTRY:', highest_earning_country)
            print('HIGHEST EARNING COUNTRY PERCENTAGE:', highest_earning_country_percentage)
        
        # Most popular occupation for those who earn >50K in India
        top_IN_occupation = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]['occupation'].value_counts().idxmax()
        if print_data:
            print('TOP OCCUPATION IN INDIA FOR THOSE EARNING >50K:', top_IN_occupation)
        
        return {
            'race_count': race_count,
            'sex_count': sex_count,
            'average_age_men': round(average_age_men, 1),
            'percentage_bachelors': round(percentage_bachelors, 1),
            'higher_education_rich': round(higher_education_rich, 1),
            'lower_education_rich': round(lower_education_rich, 1),
            'min_work_hours': min_work_hours,
            'rich_percentage': round(rich_percentage, 1),
            'highest_earning_country': highest_earning_country,
            'highest_earning_country_percentage': round(highest_earning_country_percentage, 1),
            'top_IN_occupation': top_IN_occupation
        }

    except pd.errors.EmptyDataError:
        print("Error: The file is empty. Please check the file content.")
        return None

# Run the analysis function
results = demographic_data_analyzer()
if results:
    print(results)

