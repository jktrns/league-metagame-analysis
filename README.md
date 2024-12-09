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
| `date`             | ISO-formatted date when the game was played.                                                                                                                                   |
| `side`             | The side (blue or red) the player/team was on.                                                                                                                                |
| `position`         | The role/position of the player. One of "top", "jng", "mid", "bot", "sup", or "team" (for team-level stats).                                                                  |
| `teamname`         | The name of the team.                                                                                                                                                         |
| `playername`       | The name of the player.                                                                                                                                                       |
| `champion`         | The champion played by the player.                                                                                                                                            |
| `patch`            | The patch version of the game.                                                                                                                                                |
| `pick1` - `pick5`  | The order of champions picked by the team in the draft phase. Many older games did not record this information.                                                               |
| `ban1` - `ban5`    | The order of champions banned by the team in the draft phase. Older games have a 3-ban system, while more recent games have a 5-ban system. Some games have no bans recorded. |

Here are the first 12 rows of the dataset, which represent a single game:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gameid</th>
      <th>date</th>
      <th>side</th>
      <th>position</th>
      <th>teamname</th>
      <th>playername</th>
      <th>champion</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Blue</td>
      <td>top</td>
      <td>LNG Esports</td>
      <td>Zika</td>
      <td>Aatrox</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Blue</td>
      <td>jng</td>
      <td>LNG Esports</td>
      <td>Weiwei</td>
      <td>Maokai</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Blue</td>
      <td>mid</td>
      <td>LNG Esports</td>
      <td>Scout</td>
      <td>Orianna</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Blue</td>
      <td>bot</td>
      <td>LNG Esports</td>
      <td>GALA</td>
      <td>Kalista</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Blue</td>
      <td>sup</td>
      <td>LNG Esports</td>
      <td>Mark</td>
      <td>Senna</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Red</td>
      <td>top</td>
      <td>Rare Atom</td>
      <td>Xiaoxu</td>
      <td>Rumble</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Red</td>
      <td>jng</td>
      <td>Rare Atom</td>
      <td>naiyou</td>
      <td>Rell</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Red</td>
      <td>mid</td>
      <td>Rare Atom</td>
      <td>VicLa</td>
      <td>LeBlanc</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Red</td>
      <td>bot</td>
      <td>Rare Atom</td>
      <td>Assum</td>
      <td>Varus</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Red</td>
      <td>sup</td>
      <td>Rare Atom</td>
      <td>Zorah</td>
      <td>Renata Glasc</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Blue</td>
      <td>team</td>
      <td>LNG Esports</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10660-10660_game_1</td>
      <td>2024-01-01 05:13:15</td>
      <td>Red</td>
      <td>team</td>
      <td>Rare Atom</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

We immediately notice the dataset has some glaring issues with mixed granularity and missingness, so we turn to data cleaning before we begin with exploratory data analysis.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Notice that the dataset's structure captures both macro-level game dynamics and individual player performance metrics. Each row represents a player's performance in a single game, with 10 rows per match (one for each player), plus two additional rows for team-level statistics. We will need to separate these into three datasets with the following granularity levels:

- `players`: A dataset where one row represents a single player's performance in a single game
- `teams`: A dataset where one row represents a single team's performance in a single game
- `games`: A dataset where one row represents a single game's overall statistics

The following operations were performed to clean the data:

1. We first split the dataset into `players` and `teams` DataFrames based on the `position` column.
2. We then removed all columns that were empty for all rows in both DataFrames, effectively removing columns that only pertain to one granularity level but not the other.
3. We then deal with miscellaneous cleaning, such as fixing the `dtype` of certain boolean columns alongside converting the `date` column to a `datetime` object. For the `patch` column, we pad it with zeroes to ensure sorting is consistent, and engineer a new column `major_patch` to represent the major patch version (e.g. `14.22` -> `14.X`).
4. There is a `datacompleteness` column that has three categories: "complete", "partial", and "error". The "error" category is likely due to a data collection error, so we will drop all rows with this category.
5. We also notice that a lot of the values for columns `pick1` through `pick5` are co-missing (about 24% of all rows had missing picks). In professional play, there is a "draft phase" in which the ban and pick order is actually ordinal, and alternates between the two teams with the following system:
   ![Draft Phase](https://miro.medium.com/v2/resize:fit:1386/0*YpA5_-Ti7zvcsFdd.jpg)
   The issue is that this dataset covers games from a time before this system was implemented, and as such we have many rows where the `pick1`, `pick2`, etc. columns are all missing even though their respective `champion` entries in the `players` DataFrame are not. We can't simply use the `champion` column entries as a fallback either, because the order of champions in the `champion` column is hardcoded as their role in the game: (1) top laner, (2) jungler, (3) middle laner, (4) bottom laner, and (5) support.

   To impute these missing values, what we can do is calculate the "presence" of the champion across a particular timeframe surrounding that match (in this case, we will choose the patch number), which is calculated as:

   $$
   \text{presence} = \frac{\text{number of games where champion was picked/banned}}{\text{number of games that patch was played}}
   $$

   For example, here is the presence rate of the top 10 champions in the 14.22 patch:

   | Champion  | Presence |
   |-----------|----------|
   | Corki     | 1.00     |
   | Aurora    | 1.00     |
   | Skarner   | 1.00     |
   | Ashe      | 0.94     |
   | K'Sante   | 0.88     |
   | Yone      | 0.71     |
   | Vi        | 0.65     |
   | Orianna   | 0.65     |
   | Varus     | 0.59     |
   | Jax       | 0.59     |

   We can then use this presence rate to determine the pick order of the champions. This is imperfect, but it should provide a good enough approximation for our purposes. One issue is that there are also games where the patch number is missing; however, we can intuit that the patch number is the same as the games surrounding it (since the data is sorted chronologically), so we can simply forward fill the missing patch numbers.
6. After imputing `pick1` through `pick5`, we create two columns that hold lists for the `picks` and `bans` for each game.
7. Finally, we create a DataFrame `matches` that consolidates the cleaned data into a single DataFrame where each row represents a single game. We prepend `blue_` and `red_` to columns that are team-specific to distinguish between the two teams' statistics. This allows us to analyze game-level statistics more effectively and ensures that each game's data is self-contained within a single row.

Here is the `.head()` of the `teams` DataFrame:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gameid</th>
      <th>datacompleteness</th>
      <th>league</th>
      <th>...</th>
      <th>opp_deathsat25</th>
      <th>major_patch</th>
      <th>picks</th>
      <th>bans</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>10.0</td>
      <td>03.X</td>
      <td>[Annie, Vi, Jinx, Trundle, Orianna]</td>
      <td>[Riven, Kha'Zix, Yasuo]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>4.0</td>
      <td>03.X</td>
      <td>[Thresh, LeBlanc, Lucian, Shyvana, Dr. Mundo]</td>
      <td>[Kassadin, Nidalee, Elise]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>TRLH3/44</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>7.0</td>
      <td>03.X</td>
      <td>[Elise, Lucian, Lulu, Shyvana, Kayle]</td>
      <td>[Lee Sin, Annie, Yasuo]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>TRLH3/44</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>6.0</td>
      <td>03.X</td>
      <td>[Thresh, Renekton, Caitlyn, Gragas, Vi]</td>
      <td>[Kassadin, Kha'Zix, Ziggs]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>TRLH3/76</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>3.0</td>
      <td>03.X</td>
      <td>[Thresh, Gragas, Lee Sin, Shyvana, Vayne]</td>
      <td>[Kassadin, Annie, Orianna]</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 146 columns</p>
</div>

Here is the `.head()` of the `teams` DataFrame:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gameid</th>
      <th>datacompleteness</th>
      <th>league</th>
      <th>...</th>
      <th>opp_killsat25</th>
      <th>opp_assistsat25</th>
      <th>opp_deathsat25</th>
      <th>major_patch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>03.X</td>
    </tr>
    <tr>
      <th>1</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>03.X</td>
    </tr>
    <tr>
      <th>2</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>03.X</td>
    </tr>
    <tr>
      <th>3</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>03.X</td>
    </tr>
    <tr>
      <th>4</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>03.X</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 128 columns</p>
</div>

Finally, here is the `.head()` of the `matches` DataFrame:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gameid</th>
      <th>datacompleteness</th>
      <th>league</th>
      <th>...</th>
      <th>red_opp_assistsat25</th>
      <th>red_opp_deathsat25</th>
      <th>red_picks</th>
      <th>red_bans</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>TRLH3/33</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>23.0</td>
      <td>4.0</td>
      <td>[Thresh, LeBlanc, Lucian, Shyvana, Dr. Mundo]</td>
      <td>[Kassadin, Nidalee, Elise]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>TRLH3/44</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>16.0</td>
      <td>6.0</td>
      <td>[Thresh, Renekton, Caitlyn, Gragas, Vi]</td>
      <td>[Kassadin, Kha'Zix, Ziggs]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>TRLH3/76</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>[Renekton, Vi, Leona, Ziggs, Jinx]</td>
      <td>[Yasuo, Elise, LeBlanc]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>TRLH3/85</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>36.0</td>
      <td>6.0</td>
      <td>[Lucian, Lulu, Shyvana, Olaf, Zyra]</td>
      <td>[Dr. Mundo, Yasuo, Kassadin]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>TRLH3/10072</td>
      <td>complete</td>
      <td>EU LCS</td>
      <td>...</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>[Annie, Renekton, LeBlanc, Vi, Ezreal]</td>
      <td>[Evelynn, Elise, Dr. Mundo]</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 280 columns</p>
</div>

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
