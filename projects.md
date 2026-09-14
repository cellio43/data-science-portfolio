---
title: Projects
--- 

<a href="index.html" class="home-button">← Back to Home</a>

This section documents my data science projects, research questions,
and data stories I create throughout the semesters.

## Project1 - EDA on Collegiate Wrestling

**Table of Contents:** 1. Problem Definition &nbsp;·&nbsp; 2. Data Description &nbsp;·&nbsp; 3. Data Cleaning &nbsp;·&nbsp; 4. Visualizations &nbsp;·&nbsp; 5. Limitations

<div class="toc-buttons">
  <a href="#1-problem-definition">Problem Definition</a>
  <a href="#2-data-description">Data Description</a>
  <a href="#3-data-cleaning-and-preparation">Data Cleaning</a>
  <a href="#4-visualizations-and-insights">Visualizations</a>
  <a href="#6-limitations-ethics-and-reflection">Limitations</a>
</div>

### 1. Problem Definition 
  Every year, at the end of the season, the NCAA holds a tournament for each division of wrestling. This EDA will focus on the Division 1 Wrestling NCAA Championships. At the Division 1 level of college wrestling there are 10 weight classes (125, 133, 141, 149, 157, 165, 174, 184, 197, 285). Winning the tournament is a big deal as there have only been seven 4x champions and one 5x champion. Winning an individual national championship is the ultimate goal in collegiate wrestling. So, many recruits will choose what school to commit to based on how well that school typically performs in the tournament. 
  The goal of this EDA is to address the question, "Is an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship, and are individual championships concentrated among a small number of top programs?". This question is relevant because it helps gain more insight into which programs typically perform the best at the NCAA D1 Wrestling Championships. The main stakeholders for this EDA would be recruits, their families, coaches, athletic departments, athletic directors, university administrators, the NCAA, conference bodies, sports media, and fans. 

  Recruits and their families will care about these findings because the results will have an impact on which schools recruits aim to get recruited by and ultimately where recruits decide to commit to. These findings will help recruits make this decision more confidently rather than solely relying on a University’s reputation. 

  Coaches and athletic departments, at dominant and non-dominant programs, will be interested in this question as well. Dominant programs can use these results as a recruiting tool to convince wrestlers that their university makes the most successful wrestlers. The more non-dominant programs can use the findings to analyze how far they are from top programs and it can help make decisions about what investments (coaches, facilities, etc) can help close the gap. 

  Athletic directors and university administrators that are evaluating funding at specific wrestling programs also have a stake in this question. They will want to know if success is typically spread out among lots of schools or if certain big programs typically claim the majority of titles. The answer to this question could affect how much funding is given to certain wrestling programs and to decide if the program investment is worthwhile. 

  The NCAA and conference bodies want to make sure that wrestling competitions look balanced and that lots of wrestlers from different schools have a shot at titles. Therefore, they will be interested to see if the results of the end of year tournament are predictable as this affects fan engagement, interest, media coverage, etc. 

Finally, sports media and fans will be interested to see the answer to this question because it covers exciting aspects of the sport. 

### 2. Data Description 

#### Key Variables: 
- Wrestler Name
- D1 program/school name 
- Year of tournament 
- Weight classes
- Championship outcome 

#### Here’s how I’m conceptualizing (defining) each of my variables:
- Wrestler Name: The individual who is competing in the tournament
- D1 program/school name: The specific school/university that the wrestler is representing 
- Year of tournament: The specific NCAA Championship year being recorded 
- Weight classes: The specific weight class the wrestler is competing in 
- Championship outcome: Whether the wrestler won or lost their weight class’s bracket at that year’s tournament 

#### Here’s how I’m operationalizing (measuring) each of my variables:
- Wrestler Name: A string with the wrestler’s first and last name 
- D1 program/school name: A string with the school’s name 
- Year of tournament: An int that represents the year of the tournament 
- Weight classes: An int that represents the different weight classes
- Championship outcome: Boolean where true = win and false = lose

#### What does each row represent, and what are the main features?

Each row represents a specific weight class from a specific year and who won that weight class in that year. 

#### Main features
- Wrestler: their name
- Year: year of the tournament from 1928 - 2026 (excluding 1943 - 1945; canceled due to WW2 and 2020; canceled due to Covid-19)
- School: the wrestler’s university 
- Weight class: which of the weight classes is being shown 
- Only champions are displayed for each weight class for the displayed year 


#### How large is the dataset, and what assumptions were made during collection?

  The data set is very large since it contains information about champions dating back to the tournament's inception in 1928. The main assumption that was made is through the weight-classes throughout the years. There haven’t always been 10 weight classes and the specific weight classes have changed, slightly, overtime. Therefore, the comparison isn’t exactly one-to-one in this sense. Secondly, when I am comparing the number of champions for each school I didn’t take into account that some schools may have had more appearances in the tournament or older programs than others. This affects how fairly I can compare the championship counts across different schools and highlights that viewers should take the conclusions with a grain of salt. 


### 3. Data Cleaning and Preparation

  The raw data from the National Wrestling Hall of Fame website had a table that contained the weight class, wrestler, and the wrestler's school/university for a specific year. In order to get this data into my vs code in a clean table format I implemented the following code: 

``` python
def clean_season_table(raw_df, year):
    df = raw_df.copy()
    for col in df.columns:
        df[col] = df[col].astype(str).str.replace(
            rf'^{re.escape(col)}\s*', '', regex=True, flags=re.IGNORECASE
        ).str.strip()
    df['Year'] = year
    return df
```

Then, I checked to see if any years had missing data before processing it by writing: 

```python
if len(tables) > 0 and len(tables[0]) > 0:
    df = clean_season_table(tables[0], year)
    all_champions.append(df)
else:
    years_with_no_data.append(year)
```
  This allowed me to see that 1943 - 1945 had no data. I conducted research as to why that might be and found out that those specific tournament years were canceled due to World War 2. Additionally, 2020 didn't have any data because the tournament was cancelled due to Covid-19. 

  The years 2024 - 2026 weren't included in the National Wrestling Hall of Fame website so I found them from different sources and manually entered the data rather than scraping it. Then, I made sure there were no duplicate entries and sorted the dataset chronologically by coding: 

```python
combined = combined.drop_duplicates(subset=["Year", "Weight", "Wrestler"])
combined = combined.sort_values(["Year"]).reset_index(drop=True)
``` 

#### Data Manipulation 
  I didn't end up removing any rows because each row in the dataset represents a certain individual champion and there was no invalid or incorrect information to address. The main data cleaning and transformation I addressed was converting the data into a useable format. The raw scrapped data made a table that wasn't visually appealing and contained some duplicate headings. So, I focused on fixing that. Finally, I checked for duplicates just to double check that all my data was appearing correctly especially since I combined data from automated scraping (1928 - 2023) and manual entry (2024 - 2026).   


### 4. Visualizations and Insights


<img width="800" height="auto" alt="output4" src="https://github.com/user-attachments/assets/b2c972c7-c3c3-458d-bccd-020d697c29c6" />

  
<img width="800" height="auto" alt="output3" src="https://github.com/user-attachments/assets/ff1bc349-b451-42b4-bc88-bf3081414f4f" />

  Both of the above graphs show the top 5 schools with the most individual champions, by decade just in different formats. The results illustrate that Oklahoma State dominated the NCAA D1 Wrestling Tournament from the start of the tournament, in 1928, through around 1960. The 1970s was pretty balanced among between Iowa State and Oklahoma State. From 1980 to 1990 Iowa was the clear top school for individual champions. During the 2000s Oklahoma State reclaimed the top spot. Then, from around 2010 through 2026 Penn State has absolutely dominated college wrestling with a big lead in individual champions.  


<img width="800" height="auto" alt="output2" src="https://github.com/user-attachments/assets/4d3f33e7-ac44-48a5-8711-4bf04aef5c27" />

  This graph shows the championship concentration of the top 25 schools compared to all other schools that have ever had an individual champion in the tournament. The results highlight the fact that the majority of champions are definitely concentrated among a small number of top programs. The right side of the y-axis shows the cumulative percentage of all championship and the red line on the graph helps illustrate that. Simply put, the red line tracks a running total of the percentage shown on the right side of the chart. Pick any point on the line and it tells you "counting every school up to this one, we have covered X% of all championships". The steep increase at first shows that a few top schools hold a disproportionate amount of all titles. The line flattens out towards the top of the graph because the remaining schools only add a small amount of championships to the running total. If titles were evenly distributed across all schools, then the line would be a straight diagonal. The gray dashed line marks the 80% point and shows that 19 of the top 25 schools account for 80% of all individual championships ever awarded. 

<img width="800" height="auto" alt="output" src="https://github.com/user-attachments/assets/66037aca-0d98-4524-90b8-2af734bd1b7a" />

This graph highlights the same conclusion as the previous graph, just in a different format because all the schools are plotted individually instead of showing the top 25 plus a combined "all other schools" bar. 

### 5. Storytelling and Narrative

  Using my research question, “Is there an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship, and are individual championships concentrated among a small number of top programs?”, the data I gathered, and the graphs make it easier to draw conclusions. Clearly, there is an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship. Secondly, the individual champions are largely concentrated among a small number of top programs. Since the tournament’s inauguration in 1928, Oklahoma State has had 148 champions, Iowa: 86, Iowa State: 71, Oklahoma: 67, and Penn State: 65. However, in recent years Penn State has been completely dominating the college wrestling scene. The Nittany Lions have won the NCAA D1 Wrestling Tournament team title (determined by how well each individual wrestler does) in 2011, 2012, 2013, 2014, 2016, 2017, 2018, 2019, 2022, 2023, 2024, 2025, and 2026. Additionally, since 2011 the Nittany Lions have had 44 individual champions followed by Oklahoma State’s 15, Cornell’s 14, Ohio State’s 11, and Iowa’s 8. Penn State is clearly the best NCAA D1 Wrestling Team in recent years and have had the majority of individual champions. The other top 4 schools since 2011 are Oklahoma State, Cornell, Ohio State, and Iowa. 

<img width="690" height="auto" alt="output5" src="https://github.com/user-attachments/assets/5e19cfff-191d-4738-93a3-0558dd30332f" />

This graph clearly shows Penn State's dominance in recent years as they have 29.3% of all individual champions from 2011 - 2026 compared to 7.2% from 1928 - 2026. 

### 6. Limitations, Ethics, and Reflection

#### What details does this dataset fail to capture?

  Although the conclusions I have drawn are backed by data, there is some pieces of information this study fails to capture. The most important missing pieces of data that are not considered in my analysis are the number of appearances in the tournament per team, how new or old each program is, and differences in funding, facilities, and other resources that each team gets. Additionally, I only considered if a wrestler won the tournament or lost - I didn't examine any data on wrestlers who placed but didn’t win (2nd, All-American status, etc) 
• What biases or collection gaps exist in the data?

  There isn't any bias in this data because it is either the wrestler won or they didn't. However, there are some slight collection gaps from 1943 - 1945 (no data, tournament cancelled due to World War 2) and 2020 (tournament cancelled due to Covid-19). Additionally, weight classes have changed slightly over the years as there hasn't always been 10 weight classes and the specific weight numbers have changed slightly over the years. 

#### What would you explore next if you had more time or data?

  If I had more time, I would gather data about each team’s total number of appearances and wrestlers sent to the tournament to get a better feel for what percentage of their wrestlers sent actually end up winning a title. Then I would see what percentage place compared to winning a title.

  In conclusion, there is an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship. Additionally, the individual champions are largely concentrated among a small number of top programs. The data analysis that I conducted is fairly thorough, but I there is always room for improvement. When looking at the findings of this study it should be considered that I did not include every variable possible. There are schools that very consistently produce lots of individual champions, but there are other factors that go into the wrestlers winning titles. Ultimately, it is possible to win a national title no matter what school you attend.  

## Data Sources

For 1928-2023: [nwhof.org Champions Database](https://api.nwhof.org/national-wrestling-hall-of-fame/champions-database?school=&season=1929&wrestler=)

For 2024: [NCAA.com - Penn State Wins 2024 DI Championship](https://www.ncaa.com/live-updates/wrestling-men/d1/penn-state-wins-2024-di-mens-ncaa-wrestling-championship)

For 2025: [FloWrestling - 2025 Finals Results](https://www.flowrestling.org/articles/13971188-ncaa-wrestling-championships-2025-finals-results-heres-every-champion)

For 2026: [FloWrestling - 2026 Every Champion](https://www.flowrestling.org/articles/15675254-heres-every-2026-ncaa-wrestling-champion)

