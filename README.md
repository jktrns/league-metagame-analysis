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

| Column Name  | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| `gameid`     | Unique identifier for each game                                        |
| `date`       | Date when the game was played                                          |
| `side`       | The side (blue or red) the player/team was on                          |
| `position`   | The role/position of the player (e.g., top, jungle, mid, bot, support) |
| `teamname`   | Name of the team                                                       |
| `playername` | Name of the player                                                     |
| `champion`   | Champion played by the player                                          |
| `patch`      | Patch version of the game                                              |

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
      <th>kills</th>
      <th>deaths</th>
      <th>assists</th>
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
      <td>1</td>
      <td>3</td>
      <td>1</td>
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
      <td>0</td>
      <td>4</td>
      <td>3</td>
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
      <td>0</td>
      <td>2</td>
      <td>0</td>
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
      <td>2</td>
      <td>4</td>
      <td>0</td>
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
      <td>0</td>
      <td>3</td>
      <td>3</td>
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
      <td>4</td>
      <td>0</td>
      <td>6</td>
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
      <td>1</td>
      <td>0</td>
      <td>12</td>
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
      <td>4</td>
      <td>0</td>
      <td>7</td>
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
      <td>7</td>
      <td>1</td>
      <td>5</td>
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
      <td>0</td>
      <td>2</td>
      <td>13</td>
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
      <td>3</td>
      <td>16</td>
      <td>7</td>
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
      <td>16</td>
      <td>3</td>
      <td>43</td>
    </tr>
  </tbody>
</table>
</div>

We immediately notice the dataset has some glaring issues with mixed granularity and missingness, so we turn to data cleaning before we begin with exploratory data analysis.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The following steps were taken to clean the data:

1. The data currently has two granularity levels mixed amongst the rows: one pertaining to particular players (e.g. `playername`, `champion`, `kills`, `deaths`, `assists`) and one pertaining to the team in its entirety (e.g. `teamname`, `result`, `firstblood`):
   

We will need to separate these into three datasets with the following granularity levels:
    - `players`: A dataset with rows pertaining to individual players
    - 

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
