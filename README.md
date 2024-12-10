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

| league   |     bot |     jng |     mid |     sup |     top |
|:---------|--------:|--------:|--------:|--------:|--------:|
| LCK      | 526.798 | 256.429 | 499.768 | 154.662 | 468.446 |
| LCS      | 545.215 | 277.477 | 515.228 | 146.928 | 463.346 |
| LEC      | 578.433 | 289.774 | 534.759 | 161.625 | 480.773 |
| LPL      | 575.362 | 283.195 | 525.354 | 151.095 | 491.225 |

I also created a pivot table to see if **DPM** varies between different player positions, and it does. **Bot lane** and **mid lane** consistently lead in DPM across all leagues. This makes sense because these positions typically serve as the primary damage dealers or carries for the team, which is reflected in their higher **DPM** values.

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
  src="assets/Histogram-of-DPM-per-Game-by-League2.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

To delve deeper, I separated the data by leagues and overlapped the **DPM** histograms of different leagues on top of each other. This analysis revealed that **LPL** games tend to have a higher **DPM** compared to other leagues. This could suggest that **LPL** games have more **action**, as a higher **DPM** indicates more damage is being dealt to champions, which can be a good indicator of increased action.

# Assessment of Missingness

## The Column in Question

The **'url'** column could be **Not Missing at Random (NMAR)**. This could occur if some game data are uploaded to obscure, region-specific websites that are not known to the data collector. 

The missingness in the **'url'** column could also be related to the 'league' column in a **Missing at Random (MAR)** relationship, as different regions have different policies that determine whether they have an official website and where they report the data. For instance, some regions might not have a centralized data page, or they might use different platforms to report data, which could result in missing links.

First, I plotted the distribution of league for when url is missing and when url is not missing.

**Proportion of Missing URLs (missing_url==True) Bar Graph**
<iframe
  src="assets/Histogram-of-League-(missing_url==True).html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

**Proportion of Missing URLs (missing_url==False) Bar Graph**
<iframe
  src="assets/Histogram-of-League-(missing_url==False).html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

Just by looking alone, the two distribution is very different.

## Permutation Test
I want to use a permutation test to test the following hypothesis:

### Hypothesis:
- **Null Hypothesis (H0)**: The distribution of leagues when the 'url' is missing is **the same** as the distribution of leagues when the 'url' is not missing.
- **Alternative Hypothesis (Ha)**: The distribution of leagues when the 'url' is missing is **different** from the distribution of leagues when the 'url' is not missing.

### Test Statistic:
- **Test Statistic**: **Total Variation Distance (TVD)**
- **Alpha Level**: 0.05

I used a permutation test because I want to test the difference between two distributions: the distribution of leagues when the **'url'** is **missing** versus when it is **not missing**. The **Total Variation Distance (TVD)** is appropriate here as it quantifies the difference between these two distributions. Lastly, alpha level of **0.05** is a standard choice, providing a 5% threshold for statistical significance.

## Permutation Test Results

**Empirical Distribution of TVDs**
<iframe
  src="assets/Empirical-Distribution-of-TVDs.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

The **permutation test** resulted in a **p-value close to 0**, suggests that the observed difference in the distribution of leagues when the 'url' is missing is **statistically significant**.

This implies that the missingness of the 'url' is **not Not Missing at Random (NMAR)**. Instead, the missingness of the 'url' is more likely associated with a **Missing at Random (MAR)** relationship with the league column. 

# Hypothesis Testing

Earlier during exploratory data analysis, I noticed that LPL games appear to have a higher **DPM** than other leagues, which could suggest that LPL games have more action. Now, it's time to formally test whether this observation holds true statistically.

I want to use a permutation test to test the following hypothesis:

### Hypothesis:
- **Null Hypothesis (H0)**: There is **no difference** in the mean **Damage per minute (DPM)** between **LPL** and the other tier one leagues (**LCK**, **LEC**, **LCS**).
- **Alternative Hypothesis (Ha)**: **LPL** has a **higher mean DPM** compared to the other tier one leagues (**LCK**, **LEC**, **LCS**).

### Test Statistic:
- **Test Statistic**: **Difference in mean**
- **Alpha Level**: 0.05

I used a permutation test because I want to test the difference between two distributions: the **DPM** values of **LPL** versus the **DPM** values of the other tier one leagues (**LCK, LEC, LCS**). The **difference in mean game DPM** is used as the test statistic because it allows us to see the direction of the difference — specifically, whether LPL has a **higher** mean DPM compared to the other leagues. The alpha level of **0.05** is once again the standard choice.

## Hypothesis Test Results

**Empirical Distribution of Difference in Mean DPM**
<iframe
  src="assets/Empirical-Distribution-of-Difference-in-Mean-DPM.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

The **permutation test** resulted in a **p-value close to 0**, which suggests that **LPL** games have a higher **mean DPM** than other tier one leagues (**LCK**, **LEC**, **LCS**).

This implies that there is **more action** in **LPL games** compared to the other tier one leagues. The higher **DPM** could be indicative of **more frequent or intense fights** in **LPL matches**, reflecting a more aggressive playing style.

This result align with my personal veiwing experience that **LPL** games tend to be more action-packed and exciting to watch.

# Framing a Prediction Problem

## Prediction Problem and Type

Earlier during exploratory data analysis, I noticed that **support** players **cluster** around higher **wards per minute (WPM)** values, and wondered whether **other positions** will have a **similiar clustering** in other stats.

So I want to addressing the following **classification problem**: **How can I predict the position of a player using their performance stats in the game?** This is a **multiclass classification problem** where I aim to predict the position a player in the game — **top**, **jungle**, **mid**, **bot**, or **support**. 

Since I am predicting the position of a player based on their performance, I will focus on **post-game stats**. These metrics are final values collected at the end of a match, providing a comprehensive summary of a player's contributions and performance during the game. 

### Response Variable:
- **Variable to Predict**: Player position (**top**, **jungle**, **mid**, **bot**, **support**).
- **Reason for Choice**: Players in different positions have **distinct roles** and **focus on different aspects** of the game, which is reflected their stats that vary across positions.

### Model Choice:
Given this setup, a **K-Nearest Neighbors (KNN)** model is suitable because **players with similar roles** are likely to **cluster** around similar stat values, making KNN effective in capturing these relationships.

### Evaluation Metric:
- **Metric**: **Accuracy**.
- **Reason for Choice**: While accuracy is a common metric, it is especially useful in multiclass classification when the goal is to have the **highest proportion of correctly predicted classes**. Since each game will have one of each class on each sides the class distribution is uniform, and so accuracy will give a straightforward measure of overall model performance.

# Baseline Model

I first created a very simple **K-Nearest Neighbors (KNN)** model by selecting some relevant quantitative features and using them directly without applying any modifications or transformations.

## Features

The following features are I used in this simple model:

### Damage and Gold Metrics:
- **damagetochampions**: Total damage dealt to champions.
- **dpm (damage per minute)**: Damage dealt to champions per minute.
- **damageshare**: Percentage of team damage dealt.
- **damagetakenperminute**: Damage taken per minute.
- **totalgold**: Total gold earned.
- **earnedgold**: Gold earned through gameplay.
- **earned gpm (gold per minute)**: Gold earned per minute.
- **earnedgoldshare**: Percentage of team gold earned.
- **goldspent**: Gold spent on items and upgrades.

### Vision and Ward Metrics:
- **wardsplaced**: Number of wards placed.
- **wpm (wards per minute)**: Wards placed per minute.
- **wardskilled**: Number of enemy wards destroyed.
- **wcpm (wards cleared per minute)**: Wards cleared per minute.
- **controlwardsbought**: Number of control wards purchased.
- **visionscore**: Overall vision score.
- **vspm (vision score per minute)**: Vision score per minute.

All these features are quantitative features. These 16 features covers a wide range of in-game aspects, from **damage and resource generation** to **vision control and map influence**. By covering all these diverse areas, the model can provide a **comprehensive picture of a player's play style**, enabling clear clustering by position.

## A Proof of Concept

The accuracy of this simple model is **0.60**. While this is not exceptionally high, it serves as a solid starting point and demonstrates proof of concept. It indicates that the model is capturing some meaningful patterns in the data and can differentiate between player positions to a reasonable extent. This result suggests that the approach is promising, and with further refinement—such as feature scaling, hyperparameter tuning, or incorporating additional features—the model’s performance can likely be improved.

# Final Model

## Model Improvements: K-Nearest Neighbors (KNN)
Since the **K-Nearest Neighbors (KNN)** model performed reasonably well in the baseline model, I decided to continue using it and focus on making improvements.

### Feature Standardization:
- I standardized all the features to ensure that no single column with larger values disproportionately influenced the **distance calculations** in the KNN model.
- Standardization also allowed for better measurement of a player's performance **relative to the average**, highlighting each role's specific focus more effectively.

### Hyperparameter Tuning:
- Using **GridSearchCV**, I searched for the optimal value of **n_neighbors** in the range of 1 to 30.
- The best value was found to be **20**, which balanced the trade-off between **bias** and **variance** effectively.

### Improved Accuracy:
- The accuracy of the final model increased from **0.6** in the baseline to **0.78** after these improvements—a significant enhancement.
- This improvement demonstrates that the refinements successfully captured the **nuances of player roles** and boosted the model's performance.

### Confusion Matrices

Here is the comparison of the confusion matrices for the **baseline model** and the **final model** to help with visualizing the two model's difference in performance.

**Confusion Matrix of Base Model**
<iframe
  src="assets/CM-base.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

**Confusion Matrix of Final Model**
<iframe
  src="assets/CM-final.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

For the **final model**, you can see that more predictions **land on the diagonal**, which indicates more correct predictions of player positions. This improvement reflects the impact of enhancements like feature standardization and hyperparameter tuning on the model's accuracy.