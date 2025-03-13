# Overview

In this project, I analyzed a year's worth of sales data by merging monthly sales reports into a comprehensive dataset. My analysis involved rigorous data cleaning—including handling missing values and incorrect entries—and ensuring the accuracy of data types. After cleaning and preparing the data, I investigated key business questions such as identifying the best month for sales, determining which city generated the most sales, analyzing optimal advertisement timing, understanding product bundling patterns, and highlighting the most popular products. This provided valuable insights to inform business decisions on marketing, product strategy, and sales optimization.
 
The dataset and the foundational concepts for this analysis were provided by [Keith Galli](https://www.youtube.com/watch?v=eMOA1pPVUc4&t=2s&ab_channel=KeithGalli) as part of his comprehensive data analytics tutorials, making this project both educational and practical.

# The Questions

Here are the key questions I aim to answer in this project:

1. What was the best month for sales? How much was earned that month?
2. What city had the highest number of sales? 
3. What time should we display advertisements to maximize likelihood of customers buying products?  
4. What products are most often sold together?
5. What product sold the most? Why do you think it sold the most?  

# Tools I Used

While cleaning up the data and answering the questions above, I utilized several essential tools:  

- **Python:** The backbone of this analysis, allowing me to extract and interpret meaningful trends from the dataset. I utilized the following key libraries:  
  - **Pandas:** Essential for handling, transforming, and analyzing structured data efficiently.  
  - **Matplotlib:** Used to generate clear and informative visualizations to support insights.  
- **Jupyter Notebooks:** Offered an interactive and organized environment for executing Python code while seamlessly documenting the process. 
- **Visual Studio Code:** Served as my main code editor, helping me efficiently manage and execute scripts.
- **Git & GitHub:** Played a crucial role in version control, tracking changes, and making my work accessible for collaboration and transparency. 

# Data Preparation and Cleanup

This section details the steps undertaken to prepare the data for analysis, focusing on ensuring its accuracy and usability.

## Import & Clean Up Data

I begin by importing the required libraries and loading the dataset, followed by performing initial data cleaning to ensure high data quality.

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

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To identify the most in-demand skills for the top 3 most popular data roles, I filtered the dataset to focus on the most frequently occurring positions and extracted the top 5 skills associated with each of these roles. This analysis highlights the most sought-after job titles and their corresponding key skills, providing insight into the skills to prioritize based on the role of interest.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project/2_Skills_Count.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

![Visualization of Top Skills for Data Nerds](3_Project/images/skill_demand_all_data_roles.png)

*A bar graph showing salaries for the top 3 data roles and their top 5 associated skills.*

### Insights

- SQL is the most in-demand skill for both Data Analysts and Data Scientists, appearing in over half of the job postings for each role. For Data Engineers, Python is the top skill, listed in 68% of job postings.  
- Data Engineers tend to require more specialized technical expertise, such as AWS, Azure, and Spark, whereas Data Analysts and Data Scientists are expected to excel in more general data tools like Excel and Tableau.  
- Python is a versatile skill that is highly valued across all three roles, with the highest demand among Data Scientists (72%) and Data Engineers (65%).  

## 2. How are in-demand skills trending for Data Analysts?

To analyze skill trends for Data Analysts in 2023, I filtered job postings specific to data analyst roles and grouped the skills by the posting month. This allowed me to identify the top 5 skills for data analysts each month, highlighting their popularity over the course of 2023.

You can view my notebook with detailed steps here: [3_Skills_Trend](3_Skills_Trend.ipynb).

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
````
### Results

![Trending Top Skills for Data Analysts in the US](3_Project/images/skill_trend_DA.png)  
*Bar chart showcasing the top trending skills for data analysts in the US during 2023.*

### Insights:
- SQL consistently remained the most in-demand skill throughout the year, though its demand gradually declined over time.  
- Excel saw a notable rise in demand starting in September, ultimately surpassing both Python and Tableau by year-end.  
- Python and Tableau maintained relatively stable demand throughout the year, with minor fluctuations, continuing to be key skills for data analysts. Power BI, while less in demand compared to the others, displayed a slight upward trend toward the end of the year.

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data Nerds

Check out my notebook with detailed steps here: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

#### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results

![Salary Distributions of Data Jobs in the US](3_Project/images/Salary_Distributions_of_Data_Jobs_in_the_US.png)  
*A box plot illustrating the salary distributions for the top 6 data-related job titles.*

#### Insights

- There is a notable variation in salary ranges across different job titles. Senior Data Scientist roles stand out with the highest earning potential, reaching up to $600K, highlighting the industry's high demand for advanced data expertise and experience.

- Both Senior Data Engineer and Senior Data Scientist positions exhibit a significant number of high-end outliers, indicating that exceptional skills or unique circumstances can result in substantial compensation. In comparison, Data Analyst roles show more consistent salaries with fewer outliers.

- Median salaries tend to rise with the level of seniority and specialization. Senior roles, such as Senior Data Scientist and Senior Data Engineer, not only command higher median salaries but also exhibit greater variability in earnings, reflecting the increased complexity and responsibility associated with these positions.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I refined my analysis to focus specifically on Data Analyst roles. I examined the highest-paying skills and the most in-demand skills, presenting the findings using two bar charts.

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
Here’s an overview of the highest-paying and most in-demand skills for Data Analysts in the United States:

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project/images/Highest_Paid_and_Most_In_Demand_Skills_for_Data_Analysts_in_the_US.png)

*Two distinct bar charts illustrating the highest-paying skills and the most in-demand skills for Data Analysts in the United States.*

#### Insights:

- The top chart reveals that specialized technical skills such as `dplyr`, `Bitbucket`, and `Gitlab` are linked to higher salaries, with some reaching up to $200K. This indicates that advanced technical expertise significantly boosts earning potential.

- The bottom chart shows that foundational skills like `Excel`, `PowerPoint`, and `SQL` are the most sought-after, even though they may not lead to the highest salaries. This underscores the essential role these core skills play in securing data analysis positions.

- There is a distinct difference between the skills associated with the highest salaries and those most in demand. To maximize career opportunities, data analysts should aim to build a well-rounded skill set that incorporates both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To determine the most optimal skills to learn (those that are both highly paid and in high demand), I calculated the percentage of skill demand and the median salary for these skills. This approach makes it easier to identify the top skills to focus on.

You can view my notebook with detailed steps here: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the US](3_Project/images/Most_Optimal_Skills_for_Data_Analysts_in_the_US.png)    
*A scatter plot showcasing the most optimal skills for Data Analysts in the US, highlighting those that are both high-paying and in high demand.*

#### Insights:

- The skill `Oracle` stands out with the highest median salary of nearly $97K, despite being less frequently mentioned in job postings, indicating the high value placed on specialized database expertise in the data analyst field.

- Widely required skills like `Excel` and `SQL` are prominent in job postings but tend to have lower median salaries compared to specialized skills such as `Python` and `Tableau`, which offer higher salaries and are moderately common in job listings.

- Skills like `Python`, `Tableau`, and `SQL Server` are positioned near the top of the salary range while also being fairly prevalent in job postings, suggesting that proficiency in these tools can lead to strong career opportunities in data analytics.    

### Visualizing Different Techonologies

Let’s enhance the visualization by including different technologies in the graph. We'll assign color labels based on the type of technology (e.g., {Programming: Python}).

#### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the US with Technology-Based Coloring](3_Project/images/Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_Coloring_by_Technology.png)  
*A scatter plot showcasing the most optimal skills (those that are high-paying and in high demand) for data analysts in the US, featuring color-coded labels to represent various technologies.*

#### Insights:

- The scatter plot reveals that programming skills (blue) tend to cluster at higher salary levels compared to other categories, highlighting the significant salary benefits associated with programming expertise in the data analytics field.

- Database skills (orange), such as Oracle and SQL Server, are linked to some of the highest salaries among data analyst tools, underscoring the high demand and value of data management and manipulation expertise in the industry.

- Analyst tools (green), like Tableau and Power BI, are widely mentioned in job postings and offer competitive salaries. These tools are essential for data roles, providing strong earning potential and versatility across various data tasks.



# What I Learned

This project allowed me to deepen my understanding of the data analyst job market and improve my technical skills in Python. Here are a few key takeaways:

- **Advanced Python Proficiency**: Leveraging libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and others enabled me to perform complex analyses more efficiently.  
- **Importance of Data Cleaning**: I learned that thorough data cleaning and preparation are vital to ensure accurate and reliable insights during analysis.  
- **Strategic Skill Assessment**: This project underscored the importance of aligning skills with market demand. Understanding the connection between skill demand, salary, and job availability is critical for informed career planning in tech.



# Insights

This project offered several important insights into the data analyst job market:

- **Correlation Between Skill Demand and Salary**: Skills that are in high demand, such as Python and Oracle, often command higher salaries, showcasing a clear relationship between market demand and compensation.  
- **Evolving Market Trends**: Skill demand in the data analytics field evolves, emphasizing the need to stay updated with industry trends for sustained career growth.  
- **Economic Value of Skill Development**: Identifying skills that are both in-demand and well-compensated can help data analysts prioritize their learning efforts to maximize career and financial returns.



# Challenges I Faced  

This project came with its share of challenges, offering valuable learning experiences:  

- **Data Inconsistencies**: Managing missing or inconsistent data entries required meticulous data-cleaning techniques to maintain the integrity and reliability of the analysis.  
- **Complex Data Visualization**: Creating clear and effective visualizations for complex datasets was challenging but essential for presenting insights in a compelling and understandable way.  
- **Balancing Breadth and Depth**: Striking the right balance between diving deeply into specific analyses and maintaining a broad overview of the data landscape was a constant challenge to ensure thorough coverage without losing focus.  



# Conclusion  

This analysis of the data analyst job market has been highly insightful, shedding light on the key skills and trends that define this dynamic field. The findings not only deepen my understanding but also offer actionable guidance for anyone aiming to advance in data analytics. As the industry continues to evolve, ongoing analysis will be crucial to staying competitive. This project serves as a strong foundation for future research and emphasizes the importance of continuous learning and adaptability in the data analytics profession. And last but not the least, a massive thanks to the amazing [Luke Barousse](https://www.lukebarousse.com/). Without him, this project would never exist.

