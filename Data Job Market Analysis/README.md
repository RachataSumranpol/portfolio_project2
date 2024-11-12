# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://www.lukebarousse.com/python) which provides a foundation for my analysis. Through a series of Python scripts, I explore key questions such as the most demand skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills treding for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand and High Paying)

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

* __Python:__ The backbone of my analysis, allowing me to analyze the data and find important insights. I also used the following Python libraries:
    * __Pandas Library:__ This was used to analyze the data
    * __Matplotlib Library:__ This was used to create the visualization
    * __Seaborn Library:__ Help me create more advanced visuals.
* __Jupyter Notebooks:__ The tool I used to run my Python scripts which let me easily include my notes and analysis.
* __Visual Studio Code:__ My go-to for executing my Python scripts.

# Data Preparation and Cleanup

This section outlines the step taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I started by importing necessary libraries and loading the dataset, followed by initial data cleaning task.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleaning
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)

```
## Filter US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in United States.

```python
df_US = df[df['job_country'] == 'United States']

```

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

to find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlight the most popular job titles and their top skills, showing which skills I should pay attention to depending on role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/2_Skill_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].invert_yaxis()
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)

fig.suptitle('Counts of Top Skills in Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()
```

### Results

![Visualization of Top Skills for Data Roles](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/Image/skill_demand_all_data_role.png)
*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights

- Python is a versatile skills, highly demanded across all three roles, but most prominently for Data Scientist (72%) and Data Engineer (68%).
- SQL is the most requested skill for Data Analyst and Data Scientist, with it in over half the job posting for both role. For Data Engineers, SQL is the most requested skill, appearing in 68% of job postings.
- Data Engineer require more specialized technical skills (AWS, Azure, Spark) compared to Data Analyst and Data Scientist who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

## 2. How are in-demand skills trending for Data Analyst?

to find how skills are trending in 2023 for Data Analysts, I filtered data analyst position and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023

View my notebook with detailed steps here: [3_Skill_Trend.ipynb](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/3_Skills_Trend.ipynb)

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10')

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```
## Results

![Trending Top Skills for Data Analysts in the US ](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/Image/skill_trends_DA.png)
*Bar graph visualizing the trending top skills for data analyst in the US in 2023.*

## Insights:

* SQL remains the most consistently demanded skill throughtout the year, although it shows a gradual decrease in demand.
* Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
* Both Python and Tableau show relatively stable demand throughtout the year with some fluctuations but remain essential skills for data analyst. Power Bi, while less demanded compared to the others, shows a slight upward trend towards the year's end.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary_Analysis.ipynb](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/4_Salary_Analysis.ipynb).

### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
### Results

![Salary Distributions of Data Jobs in the US](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/Image/Salary_Distributions_of_Data_Jobs_in_the_US.png)
*Box plot visualizing the salary distributions for the top 6 data job titles*

### Insights

* There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.
* Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.
* The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater varince in compensation as responsibilities increase.

### Highest Paid & Most Demanded Skills for Data Analyst

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest paid skills and the most in-demand skills. I used two bar charts to showcase thses.

#### Visualize Data

```python
fig, ax = plt.subplots(2, 1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```
#### Results

Here's the breakdown of the highest paid & most in-demand skills for data analysts in the US:

![Highest-paid&Most_in_demand_skill](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/Image/Highest_paid_and_in_demand_skill.png)

## 4. What is the most optimal skill to learn for Data Analysts?

To identify the most optimal skills to learn (the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebooks with detailed steps here: [5_Optimal_Skills.ipynb](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/5_Optimal_Skills.ipynb)

### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```

### Results

!['Most_Optimal_skill_for_DA'](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/Image/most_optimal_skill_for_DA.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

### Insights:

* The skill Oracle appears to have the highest median salary of more than $97K, despite being less common in job postings. 
* More commonly required skills like Excel and SQL have a large presence in job listings but lower median salaries compared to specialized skills like Python and Tableau, which not only have higher salaries but are also moderately prevalent in job listings.
* Skills such as Python, Tableau and SQL Server are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

## Visualizing Different Technologies

We will add color labels based on the technology

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology' # Color by technology
)
plt.show
```

### Results

![Most_Demand_Skill_For_DA_With_Coloring_Technology](https://github.com/RachataSumranpol/portfolio_project2/blob/main/Data%20Job%20Market%20Analysis/Image/most_optimal_skill_for_DA_with_coloring_by_technology.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in US with color labels for technology*

### Insights:

* The scatter plot shows that most of the programming skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefit within the data analytics field.
* The database skills (colored green), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools.
* Analyst tools (colored orange), including Tableau and Power Bi, are prevalent in job postings and offer competitive salaries

## What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

* __Advanced Python Usage__: Utilizing libraries such as Pandas for data manupilation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis task more efficiently.
* __Data Cleaning Importance__:
I learned that in-depth data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insight derived from the data.
* __Strategic Skill Analysis__:
The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

## Insights

This project provided several general insights into the data job market for analyst:

* __Skill Demand and Salary Correlation:__ There is a clear correlation between the demand for specific skill and the salaries these skills command. Skills like Python and Oracle ofter lead to higher salaries.
* __Market Trends:__ There are changing trends in skill demand. Keeping up with these trends is essential for career growth in data analytics.
* __Economic Value of Skills:__ Understanding which skills are both in-demand and well-compensated can guide in prioritizing learning to maximize their economic returns.

## Challenges I faced

This project was not without its challenges, but it provided good learning opportunities:

* __Data Inconsistencies:__ Handling missing or inconsistent data entries requires careful consideration and detailed data-cleaning techniques to ensure the integrity of the analysis.
* __Complex Data Visualization:__ Designing effective visual representations of complex datasets was challenging but critical for deliver insights clearly and compellingly.
* __Balancing Breadth and Depth:__ Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

## Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. This project is good foundations for future explorations and underscores the importance of continuous learning and adaptation in the data field.
