---
title: Projects
--- 

<a href="index.html" class="home-button">← Back to Home</a>

This section documents my data science projects, research questions,
and data stories I create throughout the semesters.

## Project1 - EDA on Collegiate Wrestling

**Table of Contents:** 1. Problem Definition &nbsp;·&nbsp; 2. Data Description &nbsp;·&nbsp; 3. Data Cleaning &nbsp;·&nbsp; 4. Visualizations &nbsp;·&nbsp; 5. Storytelling and Narrative &nbsp;·&nbsp; 6. Limitations &nbsp;·&nbsp; 7. Code and Transparency &nbsp;·&nbsp; 8. References

<div class="toc-buttons">
  <a href="#1-problem-definition">Problem Definition</a>
  <a href="#2-data-description">Data Description</a>
  <a href="#3-data-cleaning-and-preparation">Data Cleaning</a>
  <a href="#4-visualizations-and-insights">Visualizations</a>
  <a href="#5-storytelling-and-narrative">Storytelling</a>
  <a href="#6-limitations-ethics-and-reflection">Limitations</a>
  <a href="#7-code-and-transparency">Code and Transparency</a>
  <a href="#8-references">References</a>
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


<img class="wide-img" width="800" height="auto" alt="output4" src="https://github.com/user-attachments/assets/b2c972c7-c3c3-458d-bccd-020d697c29c6" />

  
<img class="wide-img" width="800" height="auto" alt="output3" src="https://github.com/user-attachments/assets/ff1bc349-b451-42b4-bc88-bf3081414f4f" />

  Both of the above graphs show the top 5 schools with the most individual champions, by decade, just in different formats. The results illustrate that Oklahoma State dominated the NCAA D1 Wrestling Tournament from the start of the tournament, in 1928, through around 1960. The 1970s was pretty balanced between Iowa State and Oklahoma State. From 1980 to 1990 Iowa was the clear top school for individual champions. During the 2000s Oklahoma State reclaimed the top spot. Then, from around 2010 through 2026 Penn State has absolutely dominated college wrestling with a big lead in individual champions.  


<img class="wide-img" width="800" height="auto" alt="output2" src="https://github.com/user-attachments/assets/4d3f33e7-ac44-48a5-8711-4bf04aef5c27" />

  This graph shows the championship concentration of the top 25 schools compared to all other schools that have ever had an individual champion in the tournament. The results highlight the fact that the majority of champions are definitely concentrated among a small number of top programs. The right side of the y-axis shows the cumulative percentage of all championship and the red line on the graph helps illustrate that. Simply put, the red line tracks a running total of the percentage shown on the right side of the chart. Pick any point on the line and it tells you "counting every school up to this one, we have covered X% of all championships". The steep increase at first shows that a few top schools hold a disproportionate amount of all titles. The line flattens out towards the top of the graph because the remaining schools only add a small amount of championships to the running total. If titles were evenly distributed across all schools, then the line would be a straight diagonal. The gray dashed line marks the 80% point and shows that 19 of the top 25 schools account for 80% of all individual championships ever awarded. 

<img class="wide-img" width="800" height="auto" alt="output" src="https://github.com/user-attachments/assets/66037aca-0d98-4524-90b8-2af734bd1b7a" />

This graph highlights the same conclusion as the previous graph, just in a different format because all the schools are plotted individually instead of showing the top 25 plus a combined "all other schools" bar. 

### 5. Storytelling and Narrative

  Using my research question, “Is there an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship, and are individual championships concentrated among a small number of top programs?”, the data I gathered, and the graphs make it easier to draw conclusions. Clearly, there is an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship. Secondly, the individual champions are largely concentrated among a small number of top programs. Since the tournament’s inauguration in 1928, Oklahoma State has had 148 champions, Iowa: 86, Iowa State: 71, Oklahoma: 67, and Penn State: 65. However, in recent years Penn State has been completely dominating the college wrestling scene. The Nittany Lions have won the NCAA D1 Wrestling Tournament team title (determined by how well each individual wrestler does) in 2011, 2012, 2013, 2014, 2016, 2017, 2018, 2019, 2022, 2023, 2024, 2025, and 2026. Additionally, since 2011 the Nittany Lions have had 44 individual champions followed by Oklahoma State’s 15, Cornell’s 14, Ohio State’s 11, and Iowa’s 8. Penn State is clearly the best NCAA D1 Wrestling Team in recent years and have had the majority of individual champions. The other top 4 schools since 2011 are Oklahoma State, Cornell, Ohio State, and Iowa. 

<img width="690" height="auto" alt="output5" src="https://github.com/user-attachments/assets/5e19cfff-191d-4738-93a3-0558dd30332f" />

This graph clearly shows Penn State's dominance in recent years as they have 29.3% of all individual champions from 2011 - 2026 compared to 7.2% from 1928 - 2026. 

### 6. Limitations, Ethics, and Reflection

#### What details does this dataset fail to capture?

  Although the conclusions I have drawn are backed by data, there are some pieces of information this study fails to capture. The most important missing pieces of data that is not considered in my analysis are the number of appearances in the tournament per team, how new or old each program is, and differences in funding, facilities, and other resources that each team gets. Additionally, I only considered if a wrestler won the tournament or lost - I didn't examine any data on wrestlers who placed but didn’t win (2nd, All-American status, etc) 

#### What biases or collection gaps exist in the data?

  There isn't any bias in this data because it is either the wrestler won or they didn't. However, there are some slight collection gaps from 1943 - 1945 (no data, tournament cancelled due to World War 2) and 2020 (tournament cancelled due to Covid-19). Additionally, weight classes have changed slightly over the years as there hasn't always been 10 weight classes and the specific weight numbers have changed slightly over the years. 

#### What would you explore next if you had more time or data?

  If I had more time, I would gather data about each team’s total number of appearances and wrestlers sent to the tournament to get a better feel for what percentage of their wrestlers sent actually end up winning a title. Then I would see what percentage place compared to winning a title.

  In conclusion, there is an association between the D1 wrestling program a wrestler attends and their likelihood of winning an individual championship. Additionally, the individual champions are largely concentrated among a small number of top programs. The data analysis that I conducted is fairly thorough, but there is always room for improvement. When looking at the findings of this study it should be considered that I did not include every variable possible. There are schools that very consistently produce lots of individual champions, but there are other factors that go into the wrestlers winning titles. Ultimately, it is possible to win a national title no matter what school you attend.  

### 7. Code and Transparency 

You can view my full code in my [GitHub repository](https://github.com/cellio43/data-science-portfolio). 

I used some generative AI to help me with this assignment and the specific generative AI that I used was Claude AI. Claude AI was used to help me with code debugging if I ran into errors. More specifically, it helped explain the errors in my code when I was trying to perform data scraping and was getting errors. Additionally, it helped me debug syntax errors and explained errors in my code when I was trying to create certain graphs, charts, etc. and was getting errors. Finally, it made some suggested changes to my code throughout the project if it thought the changes would make my data scraping more efficient or make my graphs look more visually appealing.    

## 8. References

**Peer-Reviewed Sources**

Bigsby, K. G., & Ohlmann, J. W. (2017). Ranking and prediction of collegiate wrestling. *Journal of Sports Analytics*. https://doi.org/10.3233/JSA-160024

Soyguden, A., & Ryan, T. (2026). Technical analysis of the 2023 NCAA Wrestling Championships. *Journal of ROL Sport Sciences, 7*, 1–9. https://doi.org/10.70736/jrolss.2066

Chaabene, H., Negra, Y., Bouguezzi, R., Mkaouer, B., Franchini, E., Julio, U., & Hachana, Y. (2017). Physical and physiological attributes of wrestlers: An update. *Journal of Strength and Conditioning Research, 31*(5), 1411–1442. https://doi.org/10.1519/JSC.0000000000001738

**Data Sources**

National Wrestling Hall of Fame. (n.d.). *Champions database*. Retrieved September 13, 2026, from https://api.nwhof.org/national-wrestling-hall-of-fame/champions-database?school=&season=1929&wrestler=

NCAA.com. (2024, March 23). *Penn State wins 2024 DI men's NCAA wrestling championship*. https://www.ncaa.com/live-updates/wrestling-men/d1/penn-state-wins-2024-di-mens-ncaa-wrestling-championship

FloWrestling. (2025, March 22). *NCAA wrestling championships 2025 finals results: Here's every

## Project 2 - Machine Learning on NFL Wide Recievers

**Table of Contents:** 1. Problem Definition &nbsp;·&nbsp; 2. Background and Context &nbsp;·&nbsp; 3. Data Description &nbsp;·&nbsp; 4. Data Understanding and Exploration &nbsp;·&nbsp; 5.Data Preparation and Feature Selection &nbsp;·&nbsp; 6.  Baseline and Model Development &nbsp;·&nbsp; 7.  Model Evaluation and Selection &nbsp;·&nbsp; 8. Model Interpretation and Insights &nbsp;·&nbsp; 9. Limitations, Ethics, and Reflection &nbsp;·&nbsp; 10. Code and Transparency &nbsp;·&nbsp; 11. References 

<div class="toc-buttons">
  <a href="#1-problem-definition">Problem Definition</a>
  <a href="#2-background-and-context">Background and Context</a>
  <a href="#3-data-description">Data Description</a>
  <a href="#4-data-understanding-and-exploration">Data Exploration</a>
  <a href="#5-data-preparation-and-feature-selection">Data Preparation</a>
  <a href="#6-baseline-and-model-development">Baseline and Models</a>
  <a href="#7-model-evaluation-and-selection">Model Evaluation</a>
  <a href="#8-model-interpretation-and-insights">Interpretation</a>
  <a href="#9-limitations-ethics-and-reflection">Limitations</a>
  <a href="#10-code-and-transparency">Code and Transparency</a>
  <a href="#11-references">References</a>
</div>

### 1. Problem Definition
  This project aims to predict Hall of Fame induction probability for retired NFL wide receivers based on career statistics, and to apply 2 models (a decision tree and logistic regression) to estimate which active NFL wide receivers are on pace to be inducted into the Hall of Fame. The target variable for these models would be Hall of Fame (hof) and it will be a binary variable with 1 representing HOF status while 0 represents no HOF status. This research question is a binary classification problem because the outcome is between 2 categories (inducted or not inducted). Sports media and sports analysts could benefit from my model/its predictions by using it as a starting point for wide receiver Hall of Fame debates. Fans and fantasy football users could use it to see which WRs are on track to make the Hall of Fame and base draft picks around this information. Teams and agents could use this as a benchmark throughout player's careers to see where they might need to improve to increase their chances of making the Hall of Fame. Additionally, Hall of Fame voters could use it as a reference or talking point during discussions. Finally, this problem is meaningful and worth investigating because it provides information on what normally makes a WR viewed as "Hall of Fame" caliber. However, it should be taken into consideration that this model could reflect old biases or values in what previous Hall of Fame voters thought made a Hall of Fame worthy wide receiver, so it will not be 100% accurate. 

### 2. Background and Context
  In order to understand this research question it is important to have a basic understanding of the NFL, wide receivers, and the Hall of Fame voting process. The NFL stands for the National Football League and a wide receiver is a position in football. In order for a player to be voted into the Hall of Fame they need to meet basic eligibility rules, be nominated, and then receive at least an 80% approval vote from the Selection Committee. The eligibility requirements are that the player must have been retired for at least 5 consecutive seasons, must have played at least 5 seasons in the NFL, and must have received at least one postseason honor (All-Pro selection, etc). It is also important to know that the Pro Football Hall of Fame has made some changes to the selection process which will take effect when the 2027 class is voted on. My understanding of this question was developed from my general knowledge of the NFL and my interest in statistical analysis. Through research my understanding can be supported as many people agree that NFL WR statistics such as total career receptions, total career yards, total career receiving TDs, total seasons played, avg career yards per reception, avg career yards per game, and total career 1000 yard seasons are important to examine when considering Hall of Fame status for wide receivers. 
  

### 3. Data Description 
  The data came from the nflreadpy package and from https://www.profootballhof.com/. The data from nflreadpy has one row per player per game. I turned that into a career-level table with one row per wide receiver. Each row summarizes that player's regular-season career. The dataset in terms of NFL wide receivers is very large because it contains all NFL WRs from 1999 until today. However, the dataset of NFL Hall of Fame WRs is very small because it only contains WRs in the HOF from 1999 until today. The target variable is hof which is a binary variable that is 1 if the player is in the Hall of Fame and 0 otherwise. Seven career-level features/statistics were used for the model. I examined total receptions, total receiving yards, total receiving touchdowns, seasons played, yards per reception, yards per game, and number of 1,000-yard seasons. It should be noted that the dataset does contain some restrictions/limitations. As noted before, the data starts in 1999 and only 12 Hall of Fame receivers whose careers began before 1999. Additionally, playoff games were excluded, so postseason statistics and impacts were not examined. Another limitation worth nothing is that a player will get a 0 for the hof target variable if they are not inducted as of today. Since players become eligible five 5 years after retiring they won't get in right away and many players wait for years as finalists before they are ever inducted. 

### 4. Data Understanding and Exploration 
  The summary statistics show that the typical WR is far below the top performers. The median receiver has around 363 career receptions, 4885 receiving yards, and 31 touchdowns compared to Larry Fitzgerald's 17,493 yards and 1,432 receptions. The career totals are skewed right because a small number of top players pull the averages up. Additionally, the features are on very different scales with yards in the thousands and yards per reception generally below 18. The target variable is very imbalanced because only a small number of players get inducted into the Hall of Fame. HOF players average around twice the career receptions and yards of non Hall of Famers. Matplotlib was the main tool I used to help me view the data through a bar chart, boxplot, and scatter plot. This exploration led me to use class_weight = 'balanced', a stratified train/test split to ensure that the Hall of Famers were divided proportionally, and cross validation. 

### 5. Data Preparation and Feature Selection
  Missing values appeared in certain statistics like yards per reception for players with 0 career receptions. In these cases, I removed them by requiring more than 0 receptions for active players and the players used for training needed 200 or more. The players were grouped by player_id to avoid players with the same names being merged. I kept outliers like Fitzgerald because the HOF players are what the model is trying to learn from. I included 7 career features: total receptions, total yards, total touchdowns, seasons played, yards per reception, yards per game, and 1,000-yard seasons. I ended up excluding player name and ID because they don't help predict anything. I didn't need to do any encoding because all the features are numeric. I calculated yards per reception and yards per game by diving totals. I didn't scale the features and that will not affect the random forest. However, scaling can help logistic regression so it is worth noting that my choice to not scale the statistics can be a potential limitation for the logistic regression model. The players were split 80/20 with a stratified split to ensure that both sets contained Hall of Fame players. The test set only had 2 Hall of Famers, so I evaluated the models with 5-fold cross-validation. Data leakage is prevented because each player is one row so the same player cannot appear in both the training and evaluation sets. Additionally, active players only appear at the prediction stage. Cross-validation retrains the model on each fold and is scored based only on players it hasn't seen. 

### 6. Baseline and Model Development 
  The baseline predicts "not inducted" for every player and gets 93.75% accuracy because only around 6% of receivers are Hall of Famers, but it identified none of them. This shows that the model could have very high accuracy even if it's a bad model. My models were improved to be judged on recall, precision, and f1 score. I created a random forest and logistic regression for my two models. Both models were trained on the same 7 career features. These models are appropriate because the problem is binary classification on numeric features. The random forest is a great choice because it averages many trees which helps calculate more reliable performance metrics. Logistic regression is another great option for binary classification and gives a probability of being inducted into the HOF for each player. Every model used the same features, the same players, the same class weighting, the same stratified 5-fold cross-validation, and they were evaluated the same way. 

### 7. Model Evaluation and Selection
  I used accuracy, precision, recall, and f1 score. I calculated the accuracy, but it isn't reliable since predicting "no" for every player would give 93.75% accuracy. Recall measures what share of real Hall of Famers the model found. Precision measures what share of flagged players were actually inducted. Finally, f1 score combines the two. I calculated them with 5-fold cross-validation because the single 80/20 split only had 2 Hall of Famers. Both the random forest and logistic regression performed better than the baseline on precision, recall, and f1 score. However, the final model I chose was the random forest because it had the highest precision (0.385), f1 score (0.435), and it tied with the logistic regression on recall (0.5). 

### 8. Model Interpretation and Insights 
  The models learned that Hall of Famers have much larger career production compared to non Hall of Famers. In the training data, Hall of Famers had around twice the receptions and yards and more than twice the touchdowns. Additionally, Hall of Famers had around 6 1,000 yard seasons compared to 1.7. In the random forest, yards per game was the most influential feature (0.331) and in the logistic regression model, total yards was the most influential feature. The model performs well on clear cases. Receivers with elite statistics like Moss, Owens, and Calvin Johnson are found and the forest's predictions for active players (Tyreek Hill, Jaxon Smith-Njigba, Mike Evans, etc) are reasonable predictions. It performs poorly in two areas. It misses Hall of Famers whose careers are cut off by the data (Rice, Carter, Bruce). It also flags players with Hall of Fame level statistics who aren't inducted such as Torry Holt, Reggie Wayne, and Steve Smith who were all multi-year finalists but they were never inducted. The forest's confusion matrix highlights 5 Hall of Famers found, 5 missed, and 8 false alarms out of 150 non-inductees. Therefore, the errors are divided between missing real Hall of Famers and flagging players who haven't been inducted. Feature importance illustrates which statistics the forest relied on and the coefficients show the direction of each stat's effect in the logistic regression. The predictions show that the errors are explainable because they come from truncated careers, ongoing votes, etc. Based on the models, one can conclude that career statistics definitely impact whether a NFL WR makes the Hall of Fame or not. One cannot conclude that a high probability guarantees that a player will be inducted because the model compares how closely a player's stats resemble past inductees. Predictions for players early in their careers are especially unreliable because the model trains on finished careers. 

### 9. Limitations, Ethics, and Reflection
  There is definitely a possibility for bias represented through the model. The model is trained on previous decisions made by the Selection committee which are subjective. The models could inherit these biases or tendencies of the Selection committee. Some examples of biases from the Selection committee could be era effects or favoritism towards certain players/play styles. Players and analysts would be the main people affected by incorrect predictions. A model that undervalues a player could affect their legacy. Additionally, if voters or analysts rely on the model too heavily it could continue to reflect old biases or favoritism. A false positive says that a player is on a Hall of Fame path when he may not be. Many of the false positives by the model are finalists, so the cost is small. A false negative underrepresents a player which is worse because it could cause that player to be overlooked. This model would not be appropriate for real-world decision-making because of a 0.385 precision and no award information. This makes the model too unreliable and reproduces voter bias/favoritism from the past. The next thing I would explore is adding Pro-Bowls, All-Pros, and Super Bowl wins to the data set and retrain the models. I would also add pre-1999 statistics from another source so more Hall of Famers could be used for training. Before using the model, users should understand that the probability means a current player has statistics similar to past Hall of Famers and it does not mean this player will 100% be inducted. The model has no information on awards, team success, or voter opinion and it likely reflects past voting biases. 

### 10. Code and Transparency 
You can view my full code in my [GitHub repository](https://github.com/cellio43/data-science-portfolio). 
  Throughout this project I used documentation resources from the pandas, scikit-learn, matplotlib, and nflreadpy libraries. I used some generative AI to help me with this assignment and the specific generative AI that I used was Claude AI. Claude AI was used to help me with code debugging if I ran into errors. More specifically, it helped me debug syntax errors and explained errors in my code when I was trying to train/test my models and was getting errors. 

### 11. References 

**Articles** 
Benson, J. (2025, June 29). How Pro Football Hall of Fame members are selected. Front Office Sports. https://frontofficesports.com/pro-football-hall-of-fame-selection-process/

Legwold, J. (2026, September 4). Pro Football HOF makes sweeping changes to selection process. ESPN. https://www.espn.com/nfl/story/_/id/49823476/pro-football-hof-makes-sweeping-changes-selection-process

Pro Football Hall of Fame. (n.d.). Pro Football Hall of Fame. Retrieved October 4, 2026, from https://www.profootballhof.com/

**Data Sources**
Pro Football Hall of Fame. (n.d.-a). Hall of Famers. Retrieved October 4, 2026, from https://www.profootballhof.com/players

https://github.com/nflverse/nflreadpy 




