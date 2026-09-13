[ProjectDraft.html](https://github.com/user-attachments/files/32170766/ProjectDraft.html)
# Projects
This section documents my data science projects, research questions, and data stories I create throughout the semesters.
---
## Project1 - EDA on Collegiate Wrestling 

1. Problem Definition 
  Every year, at the end of the season, the NCAA holds a tournament for each division of wrestling. This EDA will focus on the Division 1 Wrestling NCAA Championships. At the Division 1 level of college wrestling there are 10 weight classes (125, 133, 141, 149, 157, 165, 174, 184, 197, 285). Winning the tournament is a big deal as there have only been seven 4x champions and one 5x champion. Winning an individual national championship is the ultimate goal in collegiate wrestling. So, many recruits will choose what school to commit to based on how well that school typically performs in the tournament. The goal of this EDA is to address the question, "Is an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship, and are individual championships concentrated among a small number of top programs?". This question is relevant because it helps gain more insight into which programs typically perform the best at the NCAA D1 Wrestling Championships. The main stakeholders for this EDA would be recruits, their families, coaches, athletic departments, athletic directors, university administrators, the NCAA, conference bodies, sports media, and fans. 

Recruits and their families will care about these findings because the results will have an impact on which schools recruits aim to get recruited by and ultimately where recruits decide to commit to. These findings will help recruits make this decision more confidently rather than solely relying on a University’s reputation. 

Coaches and athletic departments, at dominant and non-dominant programs, will be interested in this question as well. Dominant programs can use these results as a recruiting tool to convince wrestlers that their university makes the most successful wrestlers. The more non-dominant programs can use the findings to analyze how far they are from top programs and it can help make decisions about what investments (coaches, facilities, etc) can help close the gap. 

Athletic directors and university administrators that are evaluating funding at specific wrestling programs also have a stake in this question. They will want to know if success is typically spread out among lots of schools or if certain big programs typically claim the majority of titles. The answer to this question could affect how much funding is given to certain wrestling programs and to decide if the program investment is worthwhile. 

The NCAA and conference bodies want to make sure that wrestling competitions look balanced and that lots of wrestlers from different schools have a shot at titles. Therefore, they will be interested to see if the results of the end of year tournament are predictable as this affects fan engagement, interest, media coverage, etc. 

Finally, sports media and fans will be interested to see the answer to this question because it covers exciting aspects of the sport. 

Key Variables: 
- Wrestler Name
- D1 program/school name 
- Year of tournament 
- Weight classes
- Championship outcome 

Here’s how I’m conceptualizing (defining) each of my variables:
- Wrestler Name: The individual who is competing in the tournament
- D1 program/school name: The specific school/university that the wrestler is representing 
- Year of tournament: The specific NCAA Championship year being recorded 
- Weight classes: The specific weight class the wrestler is competing in 
- Championship outcome: Whether the wrestler won or lost their weight class’s bracket at that year’s tournament 

Here’s how I’m operationalizing (measuring) each of my variables:
- Wrestler Name: A string with the wrestler’s first and last name 
- D1 program/school name: A string with the school’s name 
- Year of tournament: An int that represents the year of the tournament 
- Weight classes: An int that represents the different weight classes
- Championship outcome: Boolean where true = win and false = lose

What does each row represent, and what are the main features?

Each row represents a specific weight class from a specific year and who won that weight class in that year. 

Main features
- Wrestler: their name
- Year: year of the tournament from 1928 - 2026 (excluding 1943 - 1945; canceled due to WW2 and 2020; canceled due to Covid-19)
- School: the wrestler’s university 
- Weight class: which of the weight classes is being shown 
- Only champions are displayed for each weight class for the displayed year 


How large is the dataset, and what assumptions were made during collection?

  The data set is very large since it contains information about champions dating back to the tournament's inception in 1928. The main assumption that was made is through the weight-classes throughout the years. There haven’t always been 10 weight classes and the specific weight classes have changed, slightly, overtime. Therefore, the comparison isn’t exactly one-to-one in this sense. Secondly, when I am comparing the number of champions for each school I didn’t take into account that some schools may have had more appearances in the tournament or older programs than others. This affects how fairly I can compare the championship counts across different schools and highlights that viewers should take the conclusions with a grain of salt. 


3. Data Cleaning and Preparation

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

Explain why you removed rows, handled missing values, filtered data, or transformed

I didn't end up removing any rows because each row in the dataset represents a certain individual champion and there was no invalid or incorrect information to address. The main data cleaning and transformation I addressed was converting the data into a useable format. The raw scrapped data made a table that wasn't visually appealing and contained some duplicate headings. So, I focused on fixing that. Finally, I checked for duplicates just to double check that all my data was appearing correctly especially since I combined data from automated scraping (1928 - 2023) and manual entry (2024 - 2026).   



Where did your data come from? Provide citations and links.

For 1928- 2023
https://api.nwhof.org/national-wrestling-hall-of-fame/champions-database?school=&season=1929&wrestler= 

For 2024
https://www.ncaa.com/live-updates/wrestling-men/d1/penn-state-wins-2024-di-mens-ncaa-wrestling-championship 

For 2025 
https://www.flowrestling.org/articles/13971188-ncaa-wrestling-championships-2025-finals-results-heres-every-champion 

For 2026 
https://www.flowrestling.org/articles/15675254-heres-every-2026-ncaa-wrestling-champion 








<img width="800" height="auto" alt="output4" src="https://github.com/user-attachments/assets/b2c972c7-c3c3-458d-bccd-020d697c29c6" />
<img width="800" height="auto" alt="output3" src="https://github.com/user-attachments/assets/ff1bc349-b451-42b4-bc88-bf3081414f4f" />
<img width="800" height="auto" alt="output2" src="https://github.com/user-attachments/assets/4d3f33e7-ac44-48a5-8711-4bf04aef5c27" />
<img width="800" height="auto" alt="output" src="https://github.com/user-attachments/assets/66037aca-0d98-4524-90b8-2af734bd1b7a" />

