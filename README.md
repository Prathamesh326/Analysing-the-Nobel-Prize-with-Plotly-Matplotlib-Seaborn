# Analysing the Nobel Prize with Plotly, Matplotlib, and Seaborn

This project provides an extensive analysis of the Nobel Prize dataset using Python libraries such as Plotly, Matplotlib, and Seaborn. It explores various trends and patterns in Nobel Prize data, including countries, organizations, cities, laureates, and prize categories. The analysis is visualized with multiple types of charts such as bar charts, choropleth maps, sunburst diagrams, and line plots.

## Table of Contents
1. [Installation](#installation)
2. [Dataset](#dataset)
3. [Analysis and Visualizations](#analysis-and-visualizations)
   - [Top 20 Countries by Number of Prizes](#top-20-countries-by-number-of-prizes)
   - [Choropleth Map of Prizes by Country](#choropleth-map-of-prizes-by-country)
   - [Prize Categories by Country](#prize-categories-by-country)
   - [Nobel Prize Trends Over Time](#nobel-prize-trends-over-time)
   - [Top Research Institutions](#top-research-institutions)
   - [Prize Cities](#prize-cities)
   - [Laureate Age Analysis](#laureate-age-analysis)
4. [Conclusion](#conclusion)

## Installation

To run this project, you'll need to clone this repository and install the required Python packages. Follow the steps below:

```bash
git clone https://github.com/Prathamesh326/Analysing-the-Nobel-Prize-with-Plotly-Matplotlib-Seaborn.git
cd Analysing-the-Nobel-Prize-with-Plotly-Matplotlib-Seaborn
```

## Dataset

The Nobel Prize dataset is used for this analysis. It includes detailed information about Nobel Prize laureates, including their birth country, category, organization, and age at the time of the award.

Data used in this notebook includes the following fields:
- **birth_country_current**: The current country of birth of the laureates
- **ISO**: ISO country codes
- **organization_name**: Name of the organization
- **organization_city**: City of the laureate’s organization
- **birth_city**: Laureate's birth city
- **winning_age**: Age of the laureate at the time of winning

## Analysis and Visualizations

### Top 20 Countries by Number of Prizes
The analysis begins by exploring the top 20 countries in terms of Nobel Prizes won. The dataset is grouped by `birth_country_current` to generate the number of laureates born in each country.

**Potential Issues with the Country Data**: 
- `birth_country` vs `birth_country_current`: Some countries no longer exist or have changed names.
- `organization_country`: May differ from the laureate's country of origin.

```python
top_countries = df_data.groupby(['birth_country_current'], as_index=False).agg({'prize':pd.Series.count})
```

A horizontal bar chart is created using Plotly to visualize the distribution of prizes among the top 20 countries.

![Top 20 Countries Bar Chart](https://i.imgur.com/agcJdRS.png)

### Choropleth Map of Prizes by Country
A choropleth map is generated to show the global distribution of Nobel Prizes by country using the Plotly library. A color scale (`matter`) from Plotly's sequential color schemes is used to highlight the data.

```python
df_countries = df_data.groupby(['birth_country_current', 'ISO'], as_index=False).agg({'prize': pd.Series.count})
world_map = px.choropleth(df_countries, locations='ISO', color='prize', hover_name='birth_country_current')
```

![Choropleth Map](https://i.imgur.com/s4lqYZH.png)

### Prize Categories by Country
This part of the analysis explores the types of Nobel Prizes won by different countries, focusing on which categories dominate the prize distribution for each country. 

Germany and Japan's weakest categories and category-specific trends across nations are discussed.

```python
cat_country = df_data.groupby(['birth_country_current','category'],as_index=False).agg({'prize': pd.Series.count})
```

![Categories Bar Chart](https://i.imgur.com/iGaIKCL.png)

### Nobel Prize Trends Over Time
We examine how the number of prizes won by countries has changed over time. A line plot is created using Plotly, showcasing the cumulative number of Nobel Prizes for each country, revealing when the US became a dominant force.

```python
prize_by_year = df_data.groupby(by=['birth_country_current', 'year'], as_index=False).count()
cumulative_prizes = prize_by_year.groupby(by=['birth_country_current','year']).sum().groupby(level=[0]).cumsum()
```

![Line Chart](https://i.imgur.com/cemX4m5.png)

### Top Research Institutions
We analyze which research institutions are associated with the most Nobel Prizes. Harvard University, the University of Chicago, and other leading institutions are explored in a bar chart.

```python
top20_orgs = df_data.organization_name.value_counts()[:20]
```

![Top Institutions Bar Chart](https://i.imgur.com/zZihj2p.png)

### Prize Cities
Another interesting metric is identifying the cities where major discoveries took place. This section includes a bar chart showing the top 20 cities where Nobel laureates worked.

```python
top20_orgs_city = df_data.organization_city.value_counts()[:20]
```

![Prize Cities Bar Chart](https://i.imgur.com/cemX4m5.png)

### Laureate Age Analysis
We analyze the ages of Nobel laureates when they won the prize. Using a histogram, the distribution of laureate ages is visualized, followed by a trend analysis using a regression plot to examine if the average age has changed over time.

```python
df_data['winning_age'] = df_data.year - df_data.birth_date.dt.year
sns.histplot(data=df_data, x=df_data.winning_age, bins=30)
```

![Age Histogram](https://i.imgur.com/6HM8rfB.png)

In addition, box plots and linear regression plots are used to explore the variations in winning ages across different Nobel Prize categories.

```python
sns.boxplot(data=df_data, x='category', y='winning_age')
sns.lmplot(data=df_data, x='year', y='winning_age', hue='category', lowess=True)
```

## Conclusion
This analysis provided valuable insights into the Nobel Prize data across various dimensions: countries, categories, institutions, cities, and laureates' ages. Visualizing the data using Plotly, Matplotlib, and Seaborn revealed interesting patterns and trends, showcasing the power of data visualization in understanding global achievements.
