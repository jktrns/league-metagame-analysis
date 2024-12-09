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

|     | gameid   | datacompleteness | league | ... | deathsat25 | major_patch | picks                                         | bans                       |
| --- | -------- | ---------------- | ------ | --- | ---------- | ----------- | --------------------------------------------- | -------------------------- |
| 0   | TRLH3/33 | complete         | EU LCS | ... | 10.0       | 03.X        | [Annie, Vi, Jinx, Trundle, Orianna]           | [Riven, Kha'Zix, Yasuo]    |
| 1   | TRLH3/33 | complete         | EU LCS | ... | 4.0        | 03.X        | [Thresh, LeBlanc, Lucian, Shyvana, Dr. Mundo] | [Kassadin, Nidalee, Elise] |
| 2   | TRLH3/44 | complete         | EU LCS | ... | 7.0        | 03.X        | [Elise, Lucian, Lulu, Shyvana, Kayle]         | [Lee Sin, Annie, Yasuo]    |
| 3   | TRLH3/44 | complete         | EU LCS | ... | 6.0        | 03.X        | [Thresh, Renekton, Caitlyn, Gragas, Vi]       | [Kassadin, Kha'Zix, Ziggs] |
| 4   | TRLH3/76 | complete         | EU LCS | ... | 3.0        | 03.X        | [Thresh, Gragas, Lee Sin, Shyvana, Vayne]     | [Kassadin, Annie, Orianna] |

Here is the `.head()` of the `teams` DataFrame (with 128 columns):

|     | gameid   | datacompleteness | league | ... | killsat25 | assistsat25 | deathsat25 | major_patch |
| --- | -------- | ---------------- | ------ | --- | --------- | ----------- | ---------- | ----------- |
| 0   | TRLH3/33 | complete         | EU LCS | ... | 1.0       | 2.0         | 2.0        | 03.X        |
| 1   | TRLH3/33 | complete         | EU LCS | ... | 2.0       | 1.0         | 1.0        | 03.X        |
| 2   | TRLH3/33 | complete         | EU LCS | ... | 1.0       | 2.0         | 0.0        | 03.X        |
| 3   | TRLH3/33 | complete         | EU LCS | ... | 0.0       | 0.0         | 4.0        | 03.X        |
| 4   | TRLH3/33 | complete         | EU LCS | ... | 0.0       | 1.0         | 3.0        | 03.X        |

Finally, here is the `.head()` of the `matches` DataFrame (with 280 columns):

|     | gameid      | datacompleteness | league | ... | red_assistsat25 | red_deathsat25 | red_picks                                     | red_bans                     |
| --- | ----------- | ---------------- | ------ | --- | --------------- | -------------- | --------------------------------------------- | ---------------------------- |
| 0   | TRLH3/33    | complete         | EU LCS | ... | 23.0            | 4.0            | [Thresh, LeBlanc, Lucian, Shyvana, Dr. Mundo] | [Kassadin, Nidalee, Elise]   |
| 1   | TRLH3/44    | complete         | EU LCS | ... | 16.0            | 6.0            | [Thresh, Renekton, Caitlyn, Gragas, Vi]       | [Kassadin, Kha'Zix, Ziggs]   |
| 2   | TRLH3/76    | complete         | EU LCS | ... | 4.0             | 4.0            | [Renekton, Vi, Leona, Ziggs, Jinx]            | [Yasuo, Elise, LeBlanc]      |
| 3   | TRLH3/85    | complete         | EU LCS | ... | 36.0            | 6.0            | [Lucian, Lulu, Shyvana, Olaf, Zyra]           | [Dr. Mundo, Yasuo, Kassadin] |
| 4   | TRLH3/10072 | complete         | EU LCS | ... | 0.0             | 8.0            | [Annie, Renekton, LeBlanc, Vi, Ezreal]        | [Evelynn, Elise, Dr. Mundo]  |

### Univariate Analysis

LoL has a roster of 169 champions (with 168 available in professional play) as of November 2024, but only a subset are considered viable in professional play during any given meta. Although a bivariate analysis is more appropriate to explore meta shifts over time (putting the `major_patch` column into the x-axis), we can still get a sense of the most "reliable" champions by looking at the top 20 most picked champions across the entire dataset. I'll color the bars by role for additional context, although this is a univariate analysis and thus it cannot be used to draw any conclusions:

<iframe
  src="assets/charts/top-20-champions.html"
  width="1200"
  height="600"
  frameborder="0"
></iframe>

The data reveals Nautilus as overwhelmingly the most picked champion of all time in professional play, followed by Ezreal and Braum. Without getting too much into the particulars:
- Nautilus is a very reliable character that can provide strong engage tools and utility that allow him to set up team fights and engage targets of opportunity, which are highly valuable throughout all meta.
- Ezreal is a very difficult character with a high skill ceiling, which reaps benefits in professional play.

We can also take a look at the most banned champions across the entire dataset. In professional LoL, each team gets to ban five champions (three in older seasons) before the draft phase begins, with teams alternating bans. Banning serves several strategic purposes:

1. Target bans: Preventing a star player on the enemy team from playing their signature pick, especially if that player is known to "one-trick" that champion (i.e. they are known to play that champion very well)
2. Meta bans: Eliminating currently overpowered champions that are considered too strong in the current patch to play against. Mostly done by the blue team to immediately deny the red team a key pick
3. Composition denial: Preventing key champions that would enable certain team compositions or strategies
4. Comfort picks: Removing champions that your team struggles to play against
5. Strategic adaptation: Later bans can be used to limit the enemy team's options after seeing their initial picks

The ban phase is crucial as it sets up the draft phase and can force teams to deviate from their preferred strategies. Let's examine the most banned champions:

<iframe
  src="assets/charts/top-20-bans.html"
  width="1200"
  height="600"
  frameborder="0"
></iframe>

LeBlanc is, by far and away, the most banned champion of all time. Although these results may not be indicative of the current meta, it seems as if at some point (or consistently), LeBlanc was a non-negotiable ban in professional play.

### Bivariate Analysis

Moving onto bivariate analysis, we can take a look at the distribution of gold earned by each role across the dataset. In League of Legends, there are five primary roles that players assume, each with distinct responsibilities and strategic importance:

| Role    | Description                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Top     | The top laner is positioned in the top lane of the map. This player typically uses "tank" champions (characters that can absorb a lot of damage) or "bruisers" (characters that deal and withstand damage), who can initiate fights. They often play champions that excel in split-pushing (applying pressure on the map by attacking enemy structures while the rest of the team is elsewhere).                        |
| Jungle  | The jungler does not stay in a fixed lane but instead moves around the map, killing neutral monsters for gold and experience. This role is vital for map control, securing objectives like Dragon and Baron (powerful neutral monsters that provide team-wide benefits when defeated), and assisting other lanes by "ganking" (surprising enemy players in their lanes to help secure kills in outnumbered fights).     |
| Mid     | The mid laner occupies the central lane and is crucial for controlling the map's center. This role usually involves playing "mages" (characters that use magic to deal damage) or "assassins" (characters that can quickly eliminate opponents), champions that deal significant damage and can roam to other lanes to assist teammates in securing kills.                                                              |
| Bot     | The bottom lane consists of two players: the ADC (Attack Damage Carry, responsible for dealing consistent physical damage, especially in the late game) and the Support (provides utility, vision, and protection for the ADC). The bot lane is a key area for team coordination and strategy.                                                                                                                          |
| Support | The Support is part of the bottom lane duo and focuses on protecting the ADC. This role involves providing vision control with wards (items that reveal areas of the map), engaging or disengaging fights, and playing champions with crowd control abilities (skills that impair enemy movement or actions) and healing or shielding capabilities. The Support is essential for team fights and overall map awareness. |

We can start with the gold distribution by role. Gold distribution is a key indicator of team resource allocation strategies. Different roles have different gold requirements based on their function within the team composition, and understanding these patterns helps reveal how teams prioritize their resources:

<iframe
  src="assets/charts/gold-distribution-by-role.html"
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
