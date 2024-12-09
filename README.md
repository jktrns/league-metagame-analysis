## Introduction

### Game Overview

[League of Legends](https://www.leagueoflegends.com) (commonly abbreviated as LoL) is a [multiplayer online battle arena](https://en.wikipedia.org/wiki/Multiplayer_online_battle_arena) (MOBA) game developed and published by [Riot Games](https://riotgames.com). Released in 2009, it has become one of the most popular games in the e-sports scene, attracting tens of millions of players worldwide and maintaining a strong competitive presence. In LoL, 5 players assume the role of a "champion" with unique abilities and battle against a team of 5 other players. The game is played in matches that typically last between 20 to 50 minutes, where the primary objective is to power up your champion through strategic gameplay, acquiring items and abilities to ultimately destroy the opposing team's "Nexus," a critical structure located within their base. The game is known for its strategic depth, requiring teamwork, quick decision-making, and adaptability to the evolving in-game situations.

### Data Overview

Within this project, the data we will work with is from [Oracle's Elixir](https://oracleselixir.com), a historical database and analytics provider for the e-sports scene within LoL. This site is utilized by both professional analysts and community enthusiasts alike, offering comprehensive match data across nearly all major leagues and competitions internationally. Oracle's Elixir provides detailed statistics on player performance, team strategies, and game outcomes, making it an invaluable resource for understanding the dynamics of professional LoL play and for conducting in-depth analyses of the game and consequently, its "metagame."

[Metagame](https://en.wikipedia.org/wiki/Metagame) (which we'll refer to as "meta" for brevity) is defined as the set of strategies, tactics, and trends that are currently dominant or popular within a competitive gaming community. It encompasses the collective understanding of the most effective ways to play the game, which can evolve over time as players discover new techniques, balance changes are implemented by developers, and the competitive environment shifts. Some examples in other games include:

- In chess, the meta includes popular opening strategies like the Sicilian Defense or the Italian Game, which players study and use because they are considered effective.
- In poker, the meta could involve trends in betting strategies or bluffing techniques that are widely adopted by players to gain an edge over their opponents.
- In Monopoly, the meta might include strategies like acquiring properties that are statistically more likely to be landed on (e.g. the orange set), because they can yield a high return on investment when developed with houses and hotels.

Within the scope of LoL, the meta is primarily defined by the champions and item "builds" that are most effective in the current competitive environment's balance. This leads to the question investigated in this project:

> **How can we quantify and predict metagame shifts across different patch versions?**

Understanding and predicting metagame shifts is crucial because it directly impacts the strategies and decisions made by players, coaches, and analysts, and can provide insight as to how the development team balances the game. By staying informed about these shifts, players can maintain their competitive edge, coaches can better prepare their teams, and analysts can improve the accuracy of their game predictions.

### Data

The dataset is downloaded as a collection of `.csv` files, where each `.csv` represents one year of match data. The data covers matches from 2014 up to present day (the 2024 set updates incrementally on a daily basis). Combining all of these `.csv` files into a single dataset yields a total of 994056 rows and 168 columns. Some relevant columns include:

| Column Name        | Description                                                                                                                                                                   |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gameid`           | A unique identifier for each game. Formatting is inconsistent across leagues.                                                                                                 |
| `datacompleteness` | Whether the data is complete, partial, or contains errors.                                                                                                                    |
| `date`             | ISO-formatted date when the game was played.                                                                                                                                  |
| `side`             | The side (blue or red) the player/team was on.                                                                                                                                |
| `position`         | The role/position of the player. One of "top", "jng", "mid", "bot", "sup", or "team" (for team-level stats).                                                                  |
| `teamname`         | The name of the team.                                                                                                                                                         |
| `playername`       | The name of the player. This entry is missing for rows pertaining to teams.                                                                                                   |
| `champion`         | The champion played by the player. This entry is missing for rows pertaining to teams.                                                                                        |
| `patch`            | The patch version of the game.                                                                                                                                                |
| `pick1` - `pick5`  | The order of champions picked by the team in the draft phase. Many older games did not record this information.                                                               |
| `ban1` - `ban5`    | The order of champions banned by the team in the draft phase. Older games have a 3-ban system, while more recent games have a 5-ban system. Some games have no bans recorded. |

Here are the first 12 rows of the dataset, which represent a single game—the first 10 rows represent the players on the blue team, and the last 2 rows represent the team-level statistics. Note that many columns are not shown for brevity:

|     | gameid             | date                | side | position | teamname    | playername | champion     |
| --- | ------------------ | ------------------- | ---- | -------- | ----------- | ---------- | ------------ |
| 0   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Blue | top      | LNG Esports | Zika       | Aatrox       |
| 1   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Blue | jng      | LNG Esports | Weiwei     | Maokai       |
| 2   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Blue | mid      | LNG Esports | Scout      | Orianna      |
| 3   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Blue | bot      | LNG Esports | GALA       | Kalista      |
| 4   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Blue | sup      | LNG Esports | Mark       | Senna        |
| 5   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Red  | top      | Rare Atom   | Xiaoxu     | Rumble       |
| 6   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Red  | jng      | Rare Atom   | naiyou     | Rell         |
| 7   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Red  | mid      | Rare Atom   | VicLa      | LeBlanc      |
| 8   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Red  | bot      | Rare Atom   | Assum      | Varus        |
| 9   | 10660-10660_game_1 | 2024-01-01 05:13:15 | Red  | sup      | Rare Atom   | Zorah      | Renata Glasc |
| 10  | 10660-10660_game_1 | 2024-01-01 05:13:15 | Blue | team     | LNG Esports | NaN        | NaN          |
| 11  | 10660-10660_game_1 | 2024-01-01 05:13:15 | Red  | team     | Rare Atom   | NaN        | NaN          |

We immediately notice the dataset has some glaring issues with mixed granularity and missingness, so we turn to data cleaning before we begin with exploratory data analysis.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Since there are 12 rows per game, with 10 rows per match (one for each player), plus two additional rows for team-level statistics, we will need to separate these into three datasets with the following granularity levels:

- `players`: A dataset where one row represents a single player's performance in a single game
- `teams`: A dataset where one row represents a single team's performance in a single game
- `matches`: A dataset where one row represents a single game's overall statistics

The following operations were performed to clean the data:

1. We first split the dataset into `players` and `teams` DataFrames based on the `position` column (which is `team` for the team-level statistics).
2. We then removed all columns that were empty for all rows in both DataFrames, effectively removing columns that only pertain to one granularity level but not the other.
3. We then deal with miscellaneous cleaning, such as fixing the `dtype` of certain boolean columns alongside converting the `date` column to a `datetime` object. For the `patch` column, we pad it with zeroes to ensure sorting is consistent (e.g. `3.2` -> `03.02`,), and engineer a new column `major_patch` to represent the major patch version (e.g. `14.22` -> `14.X`).
4. There is a `datacompleteness` column that has three categories: "complete", "partial", and "error". The "error" category is likely due to a data collection error, so we will drop all rows with this category.
5. We also notice that a lot of the values for columns `pick1` through `pick5` are co-missing (about 24% of all rows had missing picks). In professional play, there is a "draft phase" in which the ban and pick order is actually ordinal, and alternates between the two teams with the following system:

   ![Draft Phase](https://miro.medium.com/v2/resize:fit:1386/0*YpA5_-Ti7zvcsFdd.jpg)

   The issue is that this dataset covers games from a time before this system was implemented, and as such we have many rows where the `pick1`, `pick2`, etc. columns are all missing even though their respective `champion` entries in the `players` DataFrame are not. We can't simply use the `champion` column entries as a fallback either, because the order of champions in the `champion` column is hardcoded as their role in the game: (1) top laner, (2) jungler, (3) middle laner, (4) bottom laner, and (5) support.

   To impute these missing values, what we can do is calculate the "presence" of the champion across a particular timeframe surrounding that match (in this case, we will choose the patch number, but you can also choose a date range or a league), which is calculated as:

   $$
   \text{presence} = \frac{\text{number of games where champion was picked/banned}}{\text{number of games that patch was played}}
   $$

   For example, here is the presence rate of the top 10 champions in the 14.22 patch:

   | Champion | Presence |
   | -------- | -------- |
   | Corki    | 1.00     |
   | Aurora   | 1.00     |
   | Skarner  | 1.00     |
   | Ashe     | 0.94     |
   | K'Sante  | 0.88     |
   | Yone     | 0.71     |
   | Vi       | 0.65     |
   | Orianna  | 0.65     |
   | Varus    | 0.59     |
   | Jax      | 0.59     |

   We can then use this presence rate to determine the pick order of the champions. This is imperfect, but it should provide a good enough approximation for our purposes. One issue is that there are also games where the patch number is missing; however, we can intuit that the patch number is the same as the games surrounding it (since the data is sorted chronologically), so we can simply forward fill the missing patch numbers.

6. After imputing `pick1` through `pick5`, we create two columns that hold lists for the `picks` and `bans` for each game.
7. Finally, we create a DataFrame `matches` that consolidates the cleaned data into a single DataFrame where each row represents a single game. We prepend `blue_` and `red_` to columns that are team-specific to distinguish between the two teams' statistics. This allows us to analyze game-level statistics more effectively and ensures that each game's data is self-contained within a single row.

Here is the `.head()` of the `teams` DataFrame (with 146 columns):

|     | gameid   | datacompleteness | league | ... | opp_deathsat25 | major_patch | picks                                         | bans                       |
| --- | -------- | ---------------- | ------ | --- | -------------- | ----------- | --------------------------------------------- | -------------------------- |
| 0   | TRLH3/33 | complete         | EU LCS | ... | 10.0           | 03.X        | [Annie, Vi, Jinx, Trundle, Orianna]           | [Riven, Kha'Zix, Yasuo]    |
| 1   | TRLH3/33 | complete         | EU LCS | ... | 4.0            | 03.X        | [Thresh, LeBlanc, Lucian, Shyvana, Dr. Mundo] | [Kassadin, Nidalee, Elise] |
| 2   | TRLH3/44 | complete         | EU LCS | ... | 7.0            | 03.X        | [Elise, Lucian, Lulu, Shyvana, Kayle]         | [Lee Sin, Annie, Yasuo]    |
| 3   | TRLH3/44 | complete         | EU LCS | ... | 6.0            | 03.X        | [Thresh, Renekton, Caitlyn, Gragas, Vi]       | [Kassadin, Kha'Zix, Ziggs] |
| 4   | TRLH3/76 | complete         | EU LCS | ... | 3.0            | 03.X        | [Thresh, Gragas, Lee Sin, Shyvana, Vayne]     | [Kassadin, Annie, Orianna] |

Here is the `.head()` of the `teams` DataFrame (with 128 columns):

|     | gameid   | datacompleteness | league | ... | opp_killsat25 | opp_assistsat25 | opp_deathsat25 | major_patch |
| --- | -------- | ---------------- | ------ | --- | ------------- | --------------- | -------------- | ----------- |
| 0   | TRLH3/33 | complete         | EU LCS | ... | 1.0           | 2.0             | 2.0            | 03.X        |
| 1   | TRLH3/33 | complete         | EU LCS | ... | 2.0           | 1.0             | 1.0            | 03.X        |
| 2   | TRLH3/33 | complete         | EU LCS | ... | 1.0           | 2.0             | 0.0            | 03.X        |
| 3   | TRLH3/33 | complete         | EU LCS | ... | 0.0           | 0.0             | 4.0            | 03.X        |
| 4   | TRLH3/33 | complete         | EU LCS | ... | 0.0           | 1.0             | 3.0            | 03.X        |

Finally, here is the `.head()` of the `matches` DataFrame (with 280 columns):

|     | gameid      | datacompleteness | league | ... | red_opp_assistsat25 | red_opp_deathsat25 | red_picks                                     | red_bans                     |
| --- | ----------- | ---------------- | ------ | --- | ------------------- | ------------------ | --------------------------------------------- | ---------------------------- |
| 0   | TRLH3/33    | complete         | EU LCS | ... | 23.0                | 4.0                | [Thresh, LeBlanc, Lucian, Shyvana, Dr. Mundo] | [Kassadin, Nidalee, Elise]   |
| 1   | TRLH3/44    | complete         | EU LCS | ... | 16.0                | 6.0                | [Thresh, Renekton, Caitlyn, Gragas, Vi]       | [Kassadin, Kha'Zix, Ziggs]   |
| 2   | TRLH3/76    | complete         | EU LCS | ... | 4.0                 | 4.0                | [Renekton, Vi, Leona, Ziggs, Jinx]            | [Yasuo, Elise, LeBlanc]      |
| 3   | TRLH3/85    | complete         | EU LCS | ... | 36.0                | 6.0                | [Lucian, Lulu, Shyvana, Olaf, Zyra]           | [Dr. Mundo, Yasuo, Kassadin] |
| 4   | TRLH3/10072 | complete         | EU LCS | ... | 0.0                 | 8.0                | [Annie, Renekton, LeBlanc, Vi, Ezreal]        | [Evelynn, Elise, Dr. Mundo]  |

### Univariate Analysis

Professional League of Legends has a roster of 169 champions (with 168 available in professional play) as of November 2024, but only a subset are considered viable in professional play during any given meta. Although a bivariate analysis is more appropriate to explore meta shifts over time (putting the `major_patch` column into the x-axis), we can still get a sense of the most "reliable" champions by looking at the top 20 most picked champions across the entire dataset. I'll color the bars by role for additional context, although this is a univariate analysis and thus it cannot be used to draw any conclusions:

<iframe
  src="assets/charts/top-20-champions.html"
  width="1200"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
