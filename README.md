# League of Legends Pro Games Analysis
by Jayden Huang
A data science project for DSC80 course in UCSD

## Introduction

League of Legend is the largest esport game in the world, and the dataset I'm using contains esports match data from the 2022 season of League of Legends. The primary goal of this project is to explore whether there are differences in performance across different professional leagues and player positions. Analyzing pro play data like this is crucial for pro play teams to develop winning strategies and  identify high-performing players. I want to see how various factors influence success in the game, and I’m particularly eager to dive into this analysis because I'm an experienced League of Legends player who has followed pro play for years.

### Key Details:
- **Number of Rows**: 150,180
- **Relevant Columns**:
  - **league**: The league in which the player competes.
  - **position**: The role or position of the player on the team.
  - **dpm**: Damage per minute, a measure of damage output.
  - **wpm**: Wards per minute, a measure of vision control in the game.

### Why Should You Care?
Understanding these metrics can provide insights into player and team strategies that could be critical for improving performance and achieving success in competitive League of Legends matches.

## Data Cleaning and Exploratory Data Analysis

In order to prepare the dataset for analysis, I took several steps to clean up the data.

### Filtering Top Tier Leagues
To concentrate on the most competitive environments, the dataset was filtered to include only games from the **four top-tier leagues**:
- **LPL** (China)
- **LCK** (Korea)
- **LEC** (Europe)
- **LCS** (North America)

Here is the first few rows and columns of the data frame:

| gameid           | datacompleteness   | url                                         | league   |   year |
|:-----------------|:-------------------|:--------------------------------------------|:---------|-------:|
| 8401-8401_game_1 | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 |
| 8401-8401_game_1 | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 |
| 8401-8401_game_1 | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 |
| 8401-8401_game_1 | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 |
| 8401-8401_game_1 | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 |

This step ensured that the analysis focuses on high-level professional play, providing insights into the strategies and performance of the best teams and players.

### Organizing Data Formats
The dataset contained a mix of **team stats** and **player stats**, which were organized differently. To address this:
- I created separate DataFrames for **team data** and **player data**, ensuring that each DataFrame included only the **relevant columns** specific to its type of data.
- This separation enabled more **targeted analysis** and avoided confusion between **team-level** and **individual-level** statistics.

Here is the first first few rows and columns of the data frame containing player data:

| position   |   damagetochampions |     dpm |   damageshare |   damagetakenperminute |
|:-----------|--------------------:|--------:|--------------:|-----------------------:|
| top        |               11188 | 491.78  |     0.279107  |                585.802 |
| jng        |                4426 | 194.549 |     0.110416  |                828.835 |
| mid        |               12577 | 552.835 |     0.313755  |                201.89  |
| bot        |                9618 | 422.769 |     0.239943  |                254.813 |
| sup        |                2276 | 100.044 |     0.0567798 |                391.912 |

**DPM vs WPM by Position Scatter Plot**
<iframe
  src="assets/Scatter-Plot-of-DPM-vs-WPM-by-Position.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

I created this scatter plots to analyze **damage per minute (DPM)** and **wards per minute (WPM)** for players in different positions. Interestingly, support players clustered distinctly from other positions along the **WPM** axis. This observation aligns with the role of support players, as vision control is a core responsibility of their position, naturally resulting in higher **WPM** values.

### Combining Data into Game-Level Summaries
To gain a broader perspective, I further aggregated **team-level** statistics into **game-level summaries**. To do this:
- I merged the two teams' data for each game into a single entry.
- I grouped the combined data by the unique 'gameid' identifier.

Here is the first first few rows and columns of the data frame containing game data:

| gameid           | league   |   game kpm |   game dpm |
|:-----------------|:---------|-----------:|-----------:|
| 8401-8401_game_1 | LPL      |     0.8351 |    3099.03 |
| 8401-8401_game_2 | LPL      |     1.2465 |    3942.17 |
| 8402-8402_game_1 | LPL      |     0.6339 |    3251.22 |
| 8402-8402_game_2 | LPL      |     0.502  |    3568.48 |
| 8402-8402_game_3 | LPL      |     0.8526 |    3633.66 |

**Damage Per Minute of Games Histogram**
<iframe
  src="assets/Histogram-of-Damage-Per-Minute-of-Games.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

I created a histogram to look at the distribution of **damage per minute (DPM)** across each game. It turns out that, similar to many other datasets, it also follows a **Normal distribution**.

**Damage Per Minute of Games by League Histogram**
<iframe
  src="assets/Histogram of DPM per Game (Damage Per Minute) by League.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

To delve deeper, I separated the data by leagues and overlapped the **DPM** histograms of different leagues on top of each other. This analysis revealed that **LPL** games tend to have a higher DPM compared to other leagues. This could suggest that **LPL** games have more **action**, as a higher **DPM** indicates more damage is being dealt to champions, which can be a good indicator of increased action.