# recommender-fantasy-football-picker
⚽️ MPG Player Recommender – Fantasy football picks optimizer Built with Python and Power BI to surface undervalued players on Mon Petit Gazon (Premier League):

**Demo 85s:** https://www.youtube.com/watch?v=ClSAfk3en2Y


**Demo with explanations:** https://www.youtube.com/watch?v=CJAY9N-eMJ4


## Context and goals

Mon Petit Gazon (MPG) is a fantasy football game created in 2011. It is played using six European leagues. The goal is to win a championship, called a "league", by building your team through an auction-based transfer market at the beginning of the game.
During each real-life football matchday, events that happen in actual games (cards, goals, fouls, injuries, etc.) are reflected in the players' performances. Each player receives a rating, and the MPG match is won by the team whose players received the best ratings that day.

The goal of this project is to conduct a data analysis to answer the following question:
"Which underrated players offer the best value for money based on their performance and cost?"

This topic was chosen because many MPG players tend to buy the same popular players and spend a large portion of their transfer budget on them. However, there are likely many lesser-known or less-hyped players who offer a better price/performance ratio.

This analysis does not take team formations into account, as they depend on each player's strategy.

We will not focus on the most famous or high-profile players, but rather on low-visibility players with strong performance and excellent value.
The objective is to determine a top 5 players per position that could be recommended to MPG players during the transfer market at the start of the game.





## Dataset

The dataset used comes from the official MPG statistics website: https://www.mpgstats.fr/, specifically for the Premier League.
The data is freely downloadable in .xls format. Two versions of the dataset are available (“New Version” and “Old Version”). The Old Version was chosen because it is more complete and contains a wider range of data. For copyright reason, the dataset won't be included.

To ensure relevant statistics and more meaningful trends, only the current season’s data was selected.

In terms of volume, the file contains 124 columns and 532 rows, corresponding to 531 players.

Before processing, the .xls file was converted into a .csv format, which is better suited for the operations and analyses to be performed.

Please note that the dataset is in French. that's why some wording are in french. I translated into english for the analysis etc but the analysis was done initially in french.

## Relevance and Feature Selection

We observed that many columns were either incomplete or irrelevant for the analysis. Therefore, the first step was to decide which variables would be useful to address our research question.

We selected the following 28 columns:

- Joueur (Player)
- Poste (Position)
- Cote (Rating)
- Var cote (Rating Change)
- Enchère moyenne (Average Bid)
- % achat (% Purchased)
- % achat tour 1 (% Purchased Round 1)
- Nombre de matchs (Matches Played)
- But (Goals)
- % titularisation (% Started)
- Temps (Minutes Played)
- Temps moyen (Average Minutes)
- Minute/But (Min per Goal)
- Prix/but (Price per Goal)
- But/Peno (Goals per Penalty)
- Passes décisives (Assists)
- Occasions créées (Chances Created)
- Tirs (Shots)
- Tirs cadrés (Shots on Target)
- Corner gagné (Corners Won)
- Ballons (Touches/balls)
- Interceptions
- Tacles (Tackles)
- %Duel gagnés (% Duels Won)
- Fautes (Fouls)
- Dégagements (Clearances)
- Ballon perdu (Balls Lost)
- Grosse occasion manquée (Big Missed Chances)

## Contextual Considerations

Football is a sport rich in statistical data, so it was important to select features aligned with our analytical goals.

Some statistics are general (e.g., minutes played), while others are position-specific (e.g., duels won, interceptions, tackles, clearances).
One of the key variables taken into account for our filtering and analysis is the player’s position, which helps contextualize the relevance of each metric.



## Pre-processing
We also had to account for columns containing null values.
Many columns had missing data, due to various factors such as player position, starting status, or player availability.
It's also important to consider that not all listed players actually play. In each real matchday, there are only 11 starters and 5 substitutes, meaning a maximum of 16 players per team, or 320 active players total per matchday.
This leaves over 200 listed players who may not feature in the game at all.

Replacing missing values with a mean or median would not be relevant here, since we are analyzing individual player performance.
Instead, we chose to replace all missing values with 0, which is meaningful in this context: a 0 either indicates no participation or a very poor performance.

We then proceeded to check and adjust the data types to ensure the variables were suitable for analysis.
A final validation was done to verify the types and to confirm that no missing values remained.

Finally, all processing was done using a new DataFrame, created from the original dataset.
This allowed us to preserve the original file and work independently on a cleaned and filtered version.


## Visualisation and statistics

In terms of data distribution, it is first interesting to analyze the distribution of player prices by position using boxplots.


<img width="945" height="563" alt="image" src="https://github.com/user-attachments/assets/eee875e2-fe34-4d79-b7c4-f62e4f6f43a7" />

We observe that player prices are unevenly distributed across positions.
Strikers show the widest price range — while most have similar values, a few outliers stand out with extremely high prices, but this concerns only a small number of players.

A similar trend is seen among attacking midfielders, though the disparities are less extreme.

As for defenders, central defenders tend to show more variation in price compared to full-backs.
Overall, defensive midfielders and central defenders are the most expensive players in their category.

Goalkeepers, on the other hand, are the cheapest and show the least price variation.
This may be due to the fact that very few goalkeepers are needed in a team, which drives prices down.

It appears that, in terms of strategy, MPG players tend to invest more heavily in midfielders and defenders.
This makes sense, since most formations typically include a higher number of midfielders and defenders compared to strikers or goalkeepers.

Two other key variables worth analyzing are the relationship between price and performance, which we will visualize using scatter plots:
- Price vs. Performance ratio:

<img width="945" height="563" alt="image" src="https://github.com/user-attachments/assets/352a4abc-9e0f-4891-8ac2-77eee0069876" />

 

When analyzing the relationship between these two variables, we can observe that some players stand out due to their high price — these are mostly strikers, but in many cases, their performance is only average or even below average.

On the other hand, many players across all positions have relatively low prices yet deliver strong overall performance.
This is especially true for goalkeepers, defenders, and some midfielders and strikers.

These observations confirm the relevance of our research question:

It is not necessary to invest in the most expensive players to achieve strong in-game performance.
Another interesting variable to analyze is overall performance by position, which will be visualized using boxplots to highlight distributions and trends.


•	Global performance by position:

 
We can observe that performance varies widely across all positions.

Strikers show the least performance variation, although a few outliers are present.

Midfielders show a similar distribution but with slightly higher variation.

Defenders, however, display a much wider range of performances.

The most significant variation is seen in goalkeepers, which is expected — goalkeeper performance has a strong impact on a match, and fewer goalkeepers are used, meaning the stats are based on a smaller sample size.

    
Now that we’ve analyzed performance distribution by position, it would be relevant to deepen the analysis by exploring the relationship between performance and playing time, using a scatter plot.


<img width="945" height="529" alt="image" src="https://github.com/user-attachments/assets/fba25312-6e0d-46ad-84c3-1c14185e05c1" />



•	PLaying time vs erformance: 

<img width="945" height="529" alt="image" src="https://github.com/user-attachments/assets/b31f78dd-1ac5-42a1-99ee-009128e2a1c1" />


We can observe that playing time and performance are not always correlated.
While the general trend suggests that more minutes played often lead to slightly better overall performance, this varies significantly depending on the player’s position.

For example, a substitute with limited minutes can still have a very high performance level, showing strong impact during short appearances.
From what we can see, very few players with high playing time actually outperform the average, and the most efficient players are not necessarily strikers, but often midfielders or defenders.

To further investigate performance, we will analyze a player’s decisive impact in relation to their price.
This will be done by comparing the number of goals and assists against the average auction price in a scatter plot, to identify the most cost-effective impactful players.



•	Goal/assist vs price:

<img width="945" height="566" alt="image" src="https://github.com/user-attachments/assets/abd436e9-2781-4ffa-aa1c-bf53defcb84b" />


The most decisive players in terms of goals and assists are predominantly strikers and attacking midfielders.

Some strikers stand out with very high prices, but there is also a large number of players with strong stats and relatively low prices.
This suggests that efficiency and price are not strongly correlated — a high price does not necessarily mean better impact.

Interestingly, several defenders appear among the top scorers and assist providers.
This raises the question of their defensive efficiency relative to their price, which would be worth exploring further using another scatter plot comparing defensive stats vs. price.



•	Interceptions/price:

<img width="945" height="572" alt="image" src="https://github.com/user-attachments/assets/4b842c6d-d841-4458-a806-b38e713a6de2" />
Once again, the emerging trend is that a defensive player’s price is not correlated with their defensive performance.
We can observe many players with relatively low prices who record a high number of interceptions, regardless of whether they are center-backs or full-backs.

This further supports the pattern observed in the previous charts:

 Price is not strongly correlated with performance.

To validate this observation, we will now create a correlation matrix to quantify the relationships between player price and various performance metrics.





•	Correlation matrix:

<img width="945" height="842" alt="image" src="https://github.com/user-attachments/assets/14a2cf56-aeac-42ff-ba64-39bfcf624808" />

This matrix provides interesting insights: overall, very few variables are strongly correlated with each other.
In particular, there is no significant correlation between player price and performance.
Many variables show either weak or even negative correlations.

As for positive correlations, they mostly concern quantitative statistics — for example:

Interceptions and minutes played

Duels won, which contribute to overall performance

Playing time, which naturally affects the number of goals and assists

This is expected: the more a player plays, the more likely they are to accumulate stats.

These findings support our main hypothesis:

A player's price is not a reliable indicator of their actual performance.
Many expensive players underperform compared to cheaper alternatives.

The final objective is to build a Top 5 list of players per position, to be recommended to MPG users.
This list can help users make more strategic, cost-effective decisions during the transfer market — especially in cases where the best-known or highest-rated players are lost in early bidding rounds.


<img width="945" height="648" alt="image" src="https://github.com/user-attachments/assets/8047ae68-8176-47a0-bd79-717eb0d2df17" />


With these matrixes by position, we have a clear view.

Starting from this, we calculate a score based on the best metrics for each position that will allow to rank the players.


**Before filtering:**

<img width="709" height="450" alt="image" src="https://github.com/user-attachments/assets/3bbb7cd5-606b-4c52-9a13-61f6c0e616a5" />



<img width="945" height="287" alt="image" src="https://github.com/user-attachments/assets/2fee7e9a-a6e6-4d3e-aa53-8d3a4df92832" />


<img width="945" height="281" alt="image" src="https://github.com/user-attachments/assets/edb814e3-91b9-45db-ac4f-b5ef52521421" />



<img width="945" height="258" alt="image" src="https://github.com/user-attachments/assets/e5e0eca2-eb82-45ea-b642-af22bf09e3f3" />



<img width="945" height="263" alt="image" src="https://github.com/user-attachments/assets/34fad9c6-b6c8-42a3-96e5-a4644d8e5fdb" />




<img width="945" height="240" alt="image" src="https://github.com/user-attachments/assets/9cea720d-145e-4c70-a0ae-8b5e21927e3d" />



<img width="945" height="245" alt="image" src="https://github.com/user-attachments/assets/db843df9-b9dd-415c-bfb6-30650ef9aa1f" />


**After filtering:**

<img width="945" height="292" alt="image" src="https://github.com/user-attachments/assets/c84b19c7-ceb0-4810-bba5-1dc088e2f10a" />




<img width="945" height="239" alt="image" src="https://github.com/user-attachments/assets/d666ee52-a161-4e08-8880-ef9cebd1eec9" />



<img width="945" height="255" alt="image" src="https://github.com/user-attachments/assets/afc50a10-8d42-4396-a55a-0fe98964dd75" />



<img width="945" height="245" alt="image" src="https://github.com/user-attachments/assets/8d9ca7e7-c685-427c-9bef-f635d88dbbf3" />



<img width="945" height="263" alt="image" src="https://github.com/user-attachments/assets/575d8740-acaa-48c2-94fb-9fb594c92514" />




<img width="945" height="230" alt="image" src="https://github.com/user-attachments/assets/ab4f0450-38db-46ac-9e35-eca418b86f13" />








## Visualisation power Bi

After determining that a player’s price does not reflect their actual performance, we will now create a visual dashboard using Power BI to recommend the top 5 players by position.

We will use the dataset that was previously cleaned and analyzed in Python — the df_clean DataFrame.
The first step involves data transformation, as many columns are recognized as "ABC" (text) in Power BI, even though they contain numeric values.
We need to convert these columns into numeric format with decimal separators to enable proper analysis.

The “Minutes” column, for example, will be formatted as whole numbers (123), not as time duration.
In football, time is typically reasoned in minutes played, not in hours, so there's no need to convert this into a time-based format.

We will also check for any errors or missing values during this transformation process.

The dashboard will help us identify the most underrated players.
Since football includes a wide variety of positions, the goal is to find underrated players within each role, based on the most relevant performance criteria for that position.
For example, the criteria for evaluating a striker are very different from those for a goalkeeper or defender.

The visualizations will begin with an overall overview, then allow users to drill down into specific positions.

The dashboard will include five separate pages:
-	The selection
-	Striker view
-	Offensive midfielder view
-	Defensive midfielder view
-	Fullback view
-	Centerback view
-	Goalkeeper view

Each page will include a summary table displaying the key statistics of players within the selected position category.
If a player is selected through any of the visual elements (charts, graphs, filters), the table will dynamically update to show all of that player’s relevant statistics simultaneously.



## Additional Files 
- The PDF file of the PowerBi dashboard.
- The jupyter notebook
